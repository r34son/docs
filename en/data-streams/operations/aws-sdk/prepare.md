---
title: "Preparing the environment in the AWS SDK in {{ yds-full-name }}"
description: "Follow this guide to configure the environment for your programming language."
---

# Preparing the environment in the AWS SDK

{% note warning %}

Currently, there is no support for the latest AWS SDK versions. Use earlier versions of the [AWS SDK](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-json-faqs.html#json-protocol-getting-started) that do not provide JSON support.

{% endnote %}

Configure the environment for your programming language:

{% list tabs group=programming_language %}

- Python {#python}

  1. Download and install [Python](https://www.python.org/downloads/) 3.6 or higher.
  1. Install the Boto3 library:

     ```bash
     pip install boto3
     ```

     To learn more about the AWS SDK for Python (Boto), see the [AWS documentation](https://aws.amazon.com/sdk-for-python/).

{% endlist %}