---
title: "Обеспечение доступа к приложению, запущенному в кластере {{ k8s }}"
---

# Обеспечение доступа к приложению, запущенному в кластере {{ k8s }}

Для предоставления доступа к приложению, запущенному в кластере {{ k8s }}, вы можете использовать публичные и внутренние [сервисы различных типов](../concepts/service.md).

Чтобы опубликовать приложение, воспользуйтесь сервисом типа `LoadBalancer`. Возможны следующие варианты:

* Публичный доступ по IP-адресу с [сетевым балансировщиком нагрузки](../../network-load-balancer/concepts/index.md).
* Доступ из внутренних сетей по IP-адресу с [внутренним сетевым балансировщиком нагрузки](../../network-load-balancer/concepts/nlb-types.md).

  Приложение будет доступно:
  * Из [подсетей](../../vpc/concepts/network.md#subnet) {{ vpc-full-name }}.
  * Из внутренних подсетей организации, подключенных к {{ yandex-cloud }} с помощью сервиса [{{ interconnect-full-name }}](../../interconnect/index.yaml).
  * Через [VPN](../../glossary/vpn.md).


Чтобы использовать защиту от DDoS, [зарезервируйте](../../vpc/operations/enable-ddos-protection.md) публичный IP-адрес и [укажите](#lb-ip) его с помощью опции `loadBalancerIP`.


{% note info %}

В отличие от IP-адреса пода или узла, который может меняться в случае обновления ресурсов группы узлов, IP-адрес сервиса типа `LoadBalancer` не изменяется.

{% endnote %}

Подготовьте и запустите в кластере {{ k8s }} приложение, к которому необходимо предоставить доступ с помощью сервиса типа `LoadBalancer`. В качестве примера используйте приложение, которое отвечает на HTTP-запросы на порт 8080.

1. [Создайте простое приложение](#simple-app).
1. [Создайте сервис типа LoadBalancer с публичным IP-адресом](#lb-create).
1. [Создайте сервис типа LoadBalancer с внутренним IP-адресом](#lb-int-create).
1. [Укажите дополнительные параметры сервиса](#advanced).
1. [Укажите параметры проверки состояния узлов](#healthcheck).
1. (Опционально) [{#T}](#network-policy).

Если созданные ресурсы вам больше не нужны, [удалите их](#clear-out).

## Перед началом работы {#before-you-begin}

Подготовьте необходимую инфраструктуру:

{% list tabs group=instructions %}

- Вручную {#manual}

  1. Создайте [облачную сеть](../../vpc/operations/network-create.md) и [подсеть](../../vpc/operations/subnet-create.md).
  1. Создайте [сервисный аккаунт](../../iam/operations/sa/create.md) с [ролью](../../iam/concepts/access-control/roles.md) `editor`.
  1. [Создайте группы безопасности](../../vpc/operations/security-group-create.md) с правилами, описанными в разделе [{#T}](connect/security-groups.md).
  1. [Создайте кластер {{ managed-k8s-name }}](kubernetes-cluster/kubernetes-cluster-create.md) и [группу узлов](node-group/node-group-create.md) с параметрами:
     * [Версия {{ k8s }}](../concepts/release-channels-and-updates.md) — 1.25 или выше.
     * Публичный доступ в интернет.

- {{ TF }} {#tf}

  1. {% include [terraform-install-without-setting](../../_includes/mdb/terraform/install-without-setting.md) %}
  1. {% include [terraform-authentication](../../_includes/mdb/terraform/authentication.md) %}
  1. {% include [terraform-setting](../../_includes/mdb/terraform/setting.md) %}
  1. {% include [terraform-configure-provider](../../_includes/mdb/terraform/configure-provider.md) %}

  1. Скачайте в ту же рабочую директорию файл конфигурации кластера {{ managed-k8s-name }} [k8s-load-balancer.tf](https://github.com/yandex-cloud/examples/blob/master/tutorials/terraform/managed-kubernetes/k8s-load-balancer.tf). В файле описаны:
     * [Сеть](../../vpc/concepts/network.md#network).
     * [Подсеть](../../vpc/concepts/network.md#subnet).
     * [Группа безопасности](../../vpc/concepts/security-groups.md) и [правила](../operations/connect/security-groups.md), необходимые для работы кластера {{ managed-k8s-name }}:
       * Правила для служебного трафика.
       * Правила для доступа к API {{ k8s }} и управления кластером {{ managed-k8s-name }} с помощью `kubectl` через порты 443 и 6443.
       * Правила для доступа к сервисам из интернета.
     * Кластер {{ managed-k8s-name }}.
     * [Сервисный аккаунт](../../iam/concepts/users/service-accounts.md), необходимый для работы кластера и [группы узлов {{ managed-k8s-name }}](../concepts/index.md#node-group).
  1. Укажите в файле конфигурации:
     * [Идентификатор каталога](../../resource-manager/operations/folder/get-id.md).
     * [Версию {{ k8s }}](../concepts/release-channels-and-updates.md) для кластера и групп узлов {{ managed-k8s-name }}.
     * Имя сервисного аккаунта кластера {{ managed-k8s-name }}.
  1. Проверьте корректность файлов конфигурации {{ TF }} с помощью команды:

     ```bash
     terraform validate
     ```

     Если в файлах конфигурации есть ошибки, {{ TF }} на них укажет.
  1. Создайте необходимую инфраструктуру:

     {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

     {% include [explore-resources](../../_includes/mdb/terraform/explore-resources.md) %}

{% endlist %}

## Создайте простое приложение {#simple-app}

1. Сохраните следующую спецификацию для создания приложения в YAML-файл с именем `hello.yaml`.

   [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) — объект API {{ k8s }}, который управляет реплицированным приложением.

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: hello
   spec:
     replicas: 2
     selector:
       matchLabels:
         app: hello
     template:
       metadata:
         labels:
           app: hello
       spec:
         containers:
         - name: hello-app
           image: {{ registry }}/crpjd37scfv6********/hello:1.1
   ```

1. Создайте приложение:

   {% list tabs group=instructions %}

   - CLI {#cli}

     {% include [cli-install](../../_includes/cli-install.md) %}

     {% include [default-catalogue](../../_includes/default-catalogue.md) %}

     ```bash
     kubectl apply -f hello.yaml
     ```

     Результат:

     ```bash
     deployment.apps/hello created
     ```

   {% endlist %}

1. Посмотрите информацию о созданном приложении:

   {% list tabs group=instructions %}

   - CLI {#cli}

     ```bash
     kubectl describe deployment hello
     ```

     Результат:

     ```text
     Name:                   hello
     Namespace:              default
     CreationTimestamp:      Wed, 28 Oct 2020 23:15:25 +0300
     Labels:                 <none>
     Annotations:            deployment.kubernetes.io/revision: 1
     Selector:               app=hello
     Replicas:               2 desired | 2 updated | 2 total | 1 available | 1 unavailable
     StrategyType:           RollingUpdate
     MinReadySeconds:        0
     RollingUpdateStrategy:  25% max unavailable, 25% max surge
     Pod Template:
       Labels:  app=hello
       Containers:
        hello-app:
         Image:        {{ registry }}/crpjd37scfv6********/hello:1.1
         Port:         <none>
         Host Port:    <none>
         Environment:  <none>
         Mounts:       <none>
       Volumes:        <none>
     Conditions:
       Type           Status  Reason
       ----           ------  ------
       Available      False   MinimumReplicasUnavailable
       Progressing    True    ReplicaSetUpdated
     OldReplicaSets:  <none>
     NewReplicaSet:   hello-******** (2/2 replicas created)
     Events:
       Type    Reason             Age   From                   Message
       ----    ------             ----  ----                   -------
       Normal  ScalingReplicaSet  10s   deployment-controller  Scaled up replica set hello-******** to 2
     ```

   {% endlist %}

## Создайте сервис типа LoadBalancer с публичным IP-адресом {#lb-create}

Когда вы создаете сервис типа `LoadBalancer`, контроллер {{ yandex-cloud }} создает в вашем каталоге и настраивает для вас [сетевой балансировщик нагрузки](../../network-load-balancer/concepts/index.md) с публичным IP-адресом.

{% note warning %}

* Созданный сетевой балансировщик тарифицируется согласно установленным [правилам тарификации](../../network-load-balancer/pricing.md).
* Не изменяйте и не удаляйте сетевой балансировщик нагрузки и целевые группы, которые будут автоматически созданы в вашем каталоге после создания сервиса с типом `LoadBalancer`.

{% endnote %}

1. Сохраните следующую спецификацию для создания сервиса типа `LoadBalancer` в YAML-файл с именем `load-balancer.yaml`:

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: hello
   spec:
     type: LoadBalancer
     ports:
     - port: 80
       name: plaintext
       targetPort: 8080
     # {{ k8s }}-метки селектора, использованные в шаблоне подов при создании объекта Deployment.
     selector:
       app: hello
   ```

   Подробнее см. в [справочнике](../../network-load-balancer/k8s-ref/service.md) ресурса `Service` для {{ network-load-balancer-full-name }}.

1. Создайте сетевой балансировщик нагрузки:

   {% list tabs group=instructions %}

   - CLI {#cli}

     ```bash
     kubectl apply -f load-balancer.yaml
     ```

     Результат:

     ```bash
     service/hello created
     ```

   {% endlist %}

1. Посмотрите информацию о созданном сетевом балансировщике нагрузки:

   {% list tabs group=instructions %}

   - Консоль управления {#console}

     1. В [консоли управления]({{ link-console-main }}) выберите ваш каталог по умолчанию.
     1. Выберите сервис **{{ ui-key.yacloud.iam.folder.dashboard.label_load-balancer }}**.
     1. На вкладке **{{ ui-key.yacloud.load-balancer.network-load-balancer.label_list }}** отображен сетевой балансировщик нагрузки с префиксом `k8s` в имени и уникальным идентификатором вашего кластера {{ k8s }} в описании.

   - CLI {#cli}

     ```bash
     kubectl describe service hello
     ```

     Результат:

     ```text
     Name:                     hello
     Namespace:                default
     Labels:                   <none>
     Annotations:              <none>
     Selector:                 app=hello
     Type:                     LoadBalancer
     IP:                       172.20.169.7
     LoadBalancer Ingress:     130.193.50.111
     Port:                     plaintext 80/TCP
     TargetPort:               8080/TCP
     NodePort:                 plaintext 32302/TCP
     Endpoints:                10.1.130.4:8080
     Session Affinity:         None
     External Traffic Policy:  Cluster
     Events:
       Type    Reason                Age    From                Message
       ----    ------                ----   ----                -------
       Normal  EnsuringLoadBalancer  2m43s  service-controller  Ensuring load balancer
       Normal  EnsuredLoadBalancer   2m17s  service-controller  Ensured load balancer
     ```

   {% endlist %}

1. Убедитесь, что приложение доступно из интернета:

   {% list tabs group=instructions %}

   - CLI {#cli}

     ```bash
     curl http://130.193.50.111
     ```

     Где `130.193.50.111` — публичный IP-адрес из поля `LoadBalancer Ingress`.

     Результат:

     ```text
     Hello, world!
     Running in 'hello-********'
     ```

   {% endlist %}

## Создайте сервис типа LoadBalancer с внутренним IP-адресом {#lb-int-create}

1. Измените спецификацию в файле `load-balancer.yaml`:

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: hello
     annotations:
       # Тип балансировщика.
       yandex.cloud/load-balancer-type: internal
       # Идентификатор подсети для внутреннего сетевого балансировщика нагрузки.
       yandex.cloud/subnet-id: e1b23q26ab1c********
   spec:
     type: LoadBalancer
     ports:
     - port: 80
       name: plaintext
       targetPort: 8080
     # {{ k8s }}-метки селектора, использованные в шаблоне подов при создании объекта Deployment.
     selector:
       app: hello
   ```

   Подробнее см. в [справочнике](../../network-load-balancer/k8s-ref/service.md#annotations) ресурса `Service` для {{ network-load-balancer-full-name }}.

1. Удалите созданный ранее внешний сетевой балансировщик нагрузки:

   {% list tabs %}

   - CLI

     ```bash
     kubectl delete service hello
     ```

     Результат:

     ```bash
     service "hello" deleted
     ```

   {% endlist %}

1. Создайте внутренний сетевой балансировщик нагрузки:

   {% list tabs %}

   - CLI

     ```bash
     kubectl apply -f load-balancer.yaml
     ```

     Результат:

     ```bash
     service/hello created
     ```

   {% endlist %}

## Укажите дополнительные параметры сервиса {#advanced}

В {{ managed-k8s-name }} для сервиса типа `LoadBalancer` можно указать дополнительные параметры:

* `loadBalancerIP` — [заранее зарезервированный статический публичный IP-адрес](../../vpc/operations/get-static-ip.md).
* `externalTrafficPolicy` — [политика управления трафиком]({{ k8s-api-link }}#servicespec-v1-core).

{% cut "Пример" %}

```yaml
apiVersion: v1
kind: Service
metadata:
  name: hello
spec:
  type: LoadBalancer
  ports:
  - port: 80
    name: plaintext
    targetPort: 8080
  selector:
    app: hello
  loadBalancerIP: 159.161.32.22
  externalTrafficPolicy: Cluster
```

{% endcut %}

Подробнее см. в [справочнике](../../network-load-balancer/k8s-ref/service.md#servicespec) ресурса `Service` для {{ network-load-balancer-full-name }}.

## Укажите параметры проверки состояния узлов {#healthcheck}

Сервисы типа `LoadBalancer` в {{ managed-k8s-name }} могут выполнять запросы проверки состояния [целевой группы](../../network-load-balancer/concepts/target-resources.md) узлов {{ k8s }}. На основании полученных метрик {{ managed-k8s-name }} принимает решение о доступности узлов.

Чтобы включить режим проверки состояния узлов, укажите аннотации `yandex.cloud/load-balancer-healthcheck-*` в спецификации сервиса, например:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: hello
  annotations:
    # Параметры проверки состояния узлов
    yandex.cloud/load-balancer-healthcheck-healthy-threshold: "2"
    yandex.cloud/load-balancer-healthcheck-interval: "2s"
```

Подробнее см. в [справочнике](../../network-load-balancer/k8s-ref/service.md#annotations) ресурса `Service` для {{ network-load-balancer-full-name }}.

## Создайте объект NetworkPolicy {#network-policy}

Для подключения к сервисам, опубликованным через {{ network-load-balancer-name }}, с определенных IP-адресов, в кластере должны быть включены [сетевые политики](../concepts/network-policy.md). Для настройки доступа через балансировщик создайте объект [NetworkPolicy]({{ k8s-api-link }}#networkpolicy-v1-networking-k8s-io) с политикой типа `Ingress`.

{% cut "Пример настройки объекта NetworkPolicy" %}

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: whitelist-netpol
  namespace: ns-example
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  ingress:
  - from:
    # Диапазоны адресов, используемые балансировщиком для проверки состояния узлов.
    - ipBlock:
        cidr: 198.18.235.0/24
    - ipBlock:
        cidr: 198.18.248.0/24
    # Диапазоны адресов подов.
    - ipBlock:
        cidr: 172.16.1.0/12
    - ipBlock:
        cidr: 172.16.2.0/12
```

{% endcut %}

Подробнее см. в [справочнике](../../network-load-balancer/k8s-ref/networkpolicy.md) ресурса `NetworkPolicy` для {{ network-load-balancer-full-name }}.

## Удалите созданные ресурсы {#clear-out}

Удалите ресурсы, которые вы больше не будете использовать, чтобы за них не списывалась плата:

{% list tabs group=instructions %}

- Вручную {#manual}

  1. [Удалите кластер {{ managed-k8s-name }}](../operations/kubernetes-cluster/kubernetes-cluster-delete.md).
  1. Если для доступа к кластеру {{ managed-k8s-name }} или узлам использовались статические [публичные IP-адреса](../../vpc/concepts/address.md#public-addresses), освободите и [удалите](../../vpc/operations/address-delete.md) их.

- {{ TF }} {#tf}

  1. В командной строке перейдите в директорию, в которой расположен актуальный конфигурационный файл {{ TF }} с планом инфраструктуры.
  1. Удалите конфигурационный файл `k8s-load-balancer.tf`.
  1. Проверьте корректность файлов конфигурации {{ TF }} с помощью команды:

     ```bash
     terraform validate
     ```

     Если в файлах конфигурации есть ошибки, {{ TF }} на них укажет.
  
  1. Подтвердите изменение ресурсов.

     {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

     Все ресурсы, которые были описаны в конфигурационном файле `k8s-load-balancer.tf`, будут удалены.

  1. Если для доступа к кластеру {{ managed-k8s-name }} или узлам использовались статические [публичные IP-адреса](../../vpc/concepts/address.md#public-addresses), освободите и [удалите](../../vpc/operations/address-delete.md) их.

{% endlist %}
