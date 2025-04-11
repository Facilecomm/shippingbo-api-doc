---
stoplight-id: kkaq6aqzizm3a
---

# Application Validation Process

## Introduction

This document outlines the validation process for applications developed using our Shippingbo API. **Please note that this validation is only required if you wish to list your application on our marketplace.** If you intend to use the API solely for your own account, validation is not necessary.

By adhering to the following guidelines, you can significantly increase the chances of a smooth and efficient validation process for your marketplace application. 

## Information Validation

* **Application Name:**
    * **Mandatory:** Must start with a capital letter and contain only alphanumeric characters and spaces.
    * **Example:** Shippingbo Tracker Pro
    * **Rule:** Must match the following regular expression: `^[A-Z][a-zA-Z0-9 ]*$`
    * **Purpose:** To ensure consistency and readability within our application catalog.
* **Logo:**
    * **Mandatory:** PNG format.
    * **Recommendation:** Appropriate size and resolution for various platforms (developer dashboard, Shippingbo marketplace, Shippingbo website and social network).
* **Description:**
    * **Mandatory:** Provided in both English and French.
    * **Criteria:** Clear, concise, and informative. Avoid using unsubstantiated superlatives, and sentence like "best application for...".
* **Links:**
    * **Mandatory:** Website, support, Terms of Service, Privacy Policy.
    * **Optional:** Status page (recommended).
* **Usage Terms:**
    * **Mandatory:** Specify whether Shippingbo usage is paid within the application.
* **Contacts:**
    * **Mandatory:** Provide marketing and technical contact information.

## Functional Validation

* **API Calls:**
    * **Mandatory:** At least one functional call per used scope.
    * **Criteria:** Calls must conform to the API documentation.
* **Webhooks:**
    * **Recommendation:** Prioritize webhooks over polling.
    * **Mandatory:** Implement the revocation webhook.
    * **Recommended:** Implement webhook hash verification (for security reasons).
* **Authentication:**
    * **Mandatory:** Functional Oauth2 implementation.

## Validation Process

* **Automated Validation:** Several checks are automated (logo format, link presence, etc.).
* **Manual Validation:** Our technical team will conduct a thorough evaluation of the functionalities and compliance with standards.

**For any questions, please join us on [Discord](https://discord.gg/2jRV2nTGyh).**