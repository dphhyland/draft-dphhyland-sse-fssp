# Banking Fraud and Scams Detection and Notification Protocol (FSDNP) Specification 1.0

## 1. Introduction

his specification defines event types and their contents based on the Shared Signals Framework [SHARED-SIGNALS-FRAMEWORK], for the real-time sharing of fraud and scam detection events among banking participants. The goal is to enhance the collective ability to detect, prevent, and respond to fraudulent activities and scams, thereby protecting customers and financial assets.

Implementers should compliment the events of this profile with those defined in OpenID RISC Profile Specification 1.0. 

### 1.1 Notational Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in BCP 14 [RFC2119] [RFC8174] when, and only when, they appear in all capitals, as shown here.

## 2. Definitions

- **Event**: Any observable occurrence in a banking system or related to a customer account that is relevant to the detection of fraud or scams.
- **Participant**: Any banking institution or financial service provider that adopts this protocol.
- **Fraud and Scam Event Message (FSEM)**: The standardized data format for communicating event information among participants.

## 3. Complex Subject Members

financial-account

    An identifier for the financial account. This identifier can be in any format, such as BSB, account number, IBAN or other account identifier.

transaction

    An identifier for the transaction. This identifier can be in any format, such as transaction ID, reference number, or other transaction identifier.

mobile-phone

    An identifier for the mobile phone number. This identifier can be in any format, such as E.164, national or other phone number format.

domain-name

    An identifier for the domain name. This identifier can be in any format, such as a fully qualified domain name (FQDN) or other domain name format.

website

    An identifier for the website. This identifier can be in any format, such as a URL or other website format.

email-address

    An identifier for the email address. This identifier can be in any format, such as an email address or other email address format.

## 3. Consideration of Privacy and AML/CFT Regulations

Participants in the FSDNP SHALL comply with all applicable data protection and privacy laws, including the General Data Protection Regulation (GDPR) and the Anti-Money Laundering and Countering the of the jurisdiction in which they operate.

Participants SHALL ensure that the exchange of FSEMs does not violate any data protection or privacy laws, and that the information shared is limited to what is necessary for the detection and prevention of fraud and scams.


## 4. Event Types

FSDNP is designed to facilitate the exchange of standardized event messages related to potential or detected fraud and scams among participants, enabling rapid and coordinated response mechanisms.

The events support changes in subject state, related to a fraud attribute of the subject or a change in the trust score for the subject.

The uri for the FSDNP is https://schemas.openid.net/secevent/fsdnp/event-type.

### 4.1 Trust Scores

Trust scores provide information on the trustworthiness of a subject, and is derived through a standardised scoring mechanism. Trust scores are used to assess the risk associated with a subject, such as a bank account, mobile phone number, email address, domain or website. 

This standard does not suggest thresholds for trust scores, as these are determined by the relying party based on their risk appetite and fraud management policies.

A Trust score of 1.0 will signal that all attributes of the trust score measure have been met, while a trust score of 0 will signal that one or more attributes that indicate fraud are present.


#### 4.1.1 Bank Account Details Check

The Bank Account details check event occurs when a bank account's name details change. 
This event accompanies the trust score for the bank account, which provides a measure of accuracy of the BSB, account number and the name of the account holder.
This check may be performed by a relying party to verify if the details they have been provided are accurate.
The check is a measure of the assurance of BSB Account Number Pair, and a closeness of the name to the account holder.

``` json

   {
     "iss": "https://idp.example.com/3456790/",
     "jti": "756E69717565206964656E746966696572",
     "iat": 1508184845,
     "aud": "https://sp.example2.net",
     "events": {
       "https://schemas.openid.net/secevent/fsdnp/event-type/bank-account-details-check": {
         "financial_account": {
           "bsb": "830123",
           "account": "DE89370400440532013000",
           "name": "John Doe"
         },
         "match-score": 0.8
       }
     }
   }

```

A match score of 1.0 indicates a perfect match across all attributes, while a score of 0.0 indicates no match. 
If there is no match on identifier values (such as BSB account number, IBAN) a match score of 0 will result. The match score is a measure of accuracy of any name details provided with the account details.

#### 4.1.2 Bank Account Trust Score

The bank account trust score provides information on the trustworthiness of a bank account. This measure is based upon the following characteristics:
- How long the account has been open. Over 12 months 1.0, 6-12 months 0.8, 3-6 months 0.6, 1-3 months 0.4, less than 1 month 0.2.
- The how recently any account details have been changed, such as account name, account holders. Within 24 hours 1.0, 24-48 hours 0.8, 48-72 hours 0.6, 72-96 hours 0.4, 96-120 hours 0.2.
- How recently the contact details for the account holder have changed. Within 24 hours 0.0, 24-48 hours 0.2, 48-72 hours 0.4, 72-96 hours 0.5, 1 week 0.8.
- If the account is actively being used. This is product dependant and will be at the discretion of the financial institution.
- Fraud has been detected on the account. If yes the trust score returned will be 0.0, if no 1.0.
- The account has been blocked by the bank. If yes the trust score returned will be 0.0, if no 1.0.

The bank account trust score will not reflect any signals relating to a Suspicious Activity Report (SAR) which would lead to any form of tipping off. 

Calculation of the trust score: TBD

A trust score of 1.0 signal that all attributes have not indicate any fraud, while a trust score of 0 will signal that one or more attributes that indicate fraud are present.

If fraud has been reported on the account it will be provided as a boolean value with the trust score.

**Weighting**
The weighting of the trust score is as follows:



``` json

   {
     "iss": "https://idp.example.com/3456790/",
     "jti": "756E69717565206964656E746966696572",
     "iat": 1508184845,
     "aud": "https://sp.example2.net",
     "events": {
       "https://schemas.openid.net/secevent/fsdnp/event-type/bank-account-trust-score": {
         "financial_account": {
           "bsb": "830123",
           "account": "DE89370400440532013000",
           "name": "John Doe"
         },
         "trust_score": 0.8,
         "fraud_detected": false
       }
     }
   }

```

#### 4.1.3 Mobile Phone Number Trust Score

A mobile phone number trust score provides information on the trustworthiness of a mobile phone number. This measure is based upon the following characteristics:
- How long the phone number has been active. Weighting: 0.5 - Over 12 months 1.0, 6-12 months 0.8, 3-6 months 0.6, 1-3 months 0.4, less than 1 month 0.2.
- If the phone number has been recently ported. Weighting: 0.2 - Within 24 hours 0.2, 24-48 hours 0.4, 48-72 hours 0.6, 72-96 hours 0.8, 96-120 hours 1.0.
- If the phone number has recently had a sim Swap. Weighting: 1.0 -  Within 24 hours 0.2, 24-48 hours 0.4, 48-72 hours 0.6, 72-96 hours 0.8, 96-120 hours 1.0.
- If the phone number has been recently used in a fraud attempt or fraud has been detected or reported. Weighting 1.0 - If yes 0.0, if no 1.0.

The trust score will not reflect any signals relating to a Suspicious Activity Report (SAR) which would lead to any form of tipping off.

Sim Swap and the detection of fraud will be provided as a boolean value with the trust score.

``` json

   {
     "iss": "https://idp.example.com/3456790/",
     "jti": "756E69717565206964656E746966696572",
     "iat": 1508184845,
     "aud": "https://sp.example2.net",
     "events": {
       "https://schemas.openid.net/secevent/fsdnp/event-type/mobile-phone-number-trust-score": {
         "phone_number": "+1234567890",
         "trust_score": 0.8,
         "sim_swap_detected": false,
          "fraud_detected": false
       }
     }
   }

``` 

#### 4.1.4 Email Address Trust Score 
  
An email address trust score provides information on the trustworthiness of an email address. This measure is based upon the following characteristics:
- How long the email address has been active.
- If the email address has been recently used in a fraud attempt or fraud has been detected or reported. If yes 0.0, if no 1.0.
- If the email address has been reported as spam. If yes 0.0, if no 1.0.
- If the email address has been reported as a phishing email address. If yes 0.0, if no 1.0.
- If the email address has been reported as a scam email address. If yes 0.0, if no 1.0.
- If the email address belongs to a domain that has been reported as high risk. If yes 0.0, if no 1.0.

  ``` json
  
    {
      "iss": "https://idp.example.com/3456790/",
      "jti": "756E69717565206964656E746966696572",
      "iat": 1508184845,
      "aud": "https://sp.example2.net",
      "events": {
        "https://schemas.openid.net/secevent/fsdnp/event-type/email-address-trust-score": {
          "email": "",  
          "trust_score": 0.8,
          "fraud_detected": false
        } 

      }
    }

  ```

#### 4.1.5 Domain Trust Score

A domain trust score provides information on the trustworthiness of a domain. This measure is based upon the following characteristics:
- How long the domain has been active.
- If the domain has been recently used in a fraud attempt or fraud has been detected or reported.
- If the domain has been used in a phishing attempt.
- If the domain comes from a high risk country.

``` json

   {
     "iss": "https://idp.example.com/3456790/",
     "jti": "756E69717565206964656E746966696572",
     "iat": 1508184845,
     "aud": "https://sp.example2.net",
     "events": {
       "https://schemas.openid.net/secevent/fsdnp/event-type/domain-trust-score": {
         "domain": "example.com",
         "trust_score": 0.8
       }
     }
   }

```

#### 4.1.6 Web Site Trust Score

A web site trust score provides information on the trustworthiness of a web site. This measure is based upon the following characteristics:
- How long the web site has been active.
- If the web site has been recently used in a fraud attempt or fraud has been detected or reported.
- If the web site has been used in a phishing attempt.
- If the web site comes from a high risk country.

``` json

   {
     "iss": "https://idp.example.com/3456790/",
     "jti": "756E69717565206964656E746966696572",
     "iat": 1508184845,
     "aud": "https://sp.example2.net",
     "events": {
       "https://schemas.openid.net/secevent/fsdnp/event-type/web-site-trust-score": {
         "url": "https://example.com",
         "trust_score": 0.8
       }
     }
   }

``` 

### 4.2 Fraud and Scam Signals


This specification defines the following event types that may be used to signal to subscribers that a fraud or scam event has been detected, or a significant change in the trust score of a subject has occurred.


#### 4.2.1 Bank Account Fraud Detected

Event type for fraud detected on a financial account, transaction, subject or other resource.

``` json

   {
     "iss": "https://idp.example.com/3456790/",
     "jti": "756E69717565206964656E746966696572",
     "iat": 1508184845,
     "aud": "https://sp.example2.net",
     "events": {
       "https://schemas.openid.net/secevent/fsdnp/event-type/unsual-activities": {
         "financial_account": {
           "bsb": "830123",
           "account": "DE89370400440532013000",
           "name": "John Doe"
         },
       }
     }
   }

```

### 4.2.2 Mobile Phone Number Fraud Detected 

Event type for fraud detected on a financial account, transaction, subject or other resource.

Resource Types: 
- financial account: fraud has been detected on a financial account. 
- transaction: fraud has been detected for a transaction with identifier

``` json

   {
     "iss": "https://idp.example.com/3456790/",
     "jti": "756E69717565206964656E746966696572",
     "iat": 1508184845,
     "aud": "https://sp.example2.net",
     "events": {
       "https://schemas.openid.net/secevent/fsdnp/event-type/fraud-detected": {
         "mobile-phone": {
           "number": "+61456234545"

         },
       }
     }
   }

```

### 4.2.3 Mobile Phone SIM Swap Detected

``` json

   {
     "iss": "https://idp.example.com/3456790/",
     "jti": "756E69717565206964656E746966696572",
     "iat": 1508184845,
     "aud": "https://sp.example2.net",
     "events": {
       "https://schemas.openid.net/secevent/fsdnp/event-type/fraud-detected": {
         "mobile-phone": {
           "number": "+61456234545"

         },
       }
     }
   }


### 4.2.4 Email Address Fraud Detected

### 4.2.5 Website Phising Detected

### 4.2.6 Domain Phising Detected


### 4.2.7 Phishing Attempts Detected

Event raised when a party attempts a phishing attack on a user account.
sub in this case would be a PPID known by the RP as defined with OIDC ... (enter PPID reference here)

``` json

 {
  "iss": "https://idp.example.com/",
  "jti": "756E69717565206964656E746966696572",
  "iat": 1508184845,
  "aud": "636C69656E745F6964",
  "events": {
    "https://schemas.openid.net/secevent/risc/event-type/\
    phishing-detected": {
      "subject": {
        "format": "iss_sub",
        "iss": "https://www.wbank.com/",
        "sub": "7375626A656374",
      }
    }
  }
}

```


## 4.4 High-Risk Transactions

Event type raised high risk transactions are performed on a financial account. 

``` json

 {
  "iss": "https://idp.example.com/",
  "jti": "756E69717565206964656E746966696572",
  "iat": 1508184845,
  "aud": "636C69656E745F6964",
  "events": {
    "https://schemas.openid.net/secevent/risc/event-type/\
    phishing-detected": {
      "subject": {
        "format": "iss_sub",
        "iss": "https://www.wbank.com/",
        "sub": "7375626A656374",
      }
    }
  }
}

```




### 5 Data Protection and Privacy

Participants MUST ensure that all FSEMs comply with applicable data protection and privacy laws. Measures such as pseudonymization of customer identifiers and encryption of event messages SHALL be implemented.

## 6. Operational Requirements

Participants in the FSDNP SHALL:
- Establish secure communication channels for the exchange of FSEMs.
- Implement mechanisms to verify the authenticity of FSEMs received from other participants.
- Respond to FSEMs in accordance with their internal fraud management and customer protection policies.

## 6. Security Considerations

Participants MUST implement appropriate security measures to protect the integrity and confidentiality of FSEMs and to prevent unauthorized access to sensitive information.

## 7. Compliance and Reporting

Participants are encouraged to report on the effectiveness of the FSDNP in detecting and preventing fraud and scams, contributing to the continuous improvement of the protocol.

# Normative References

[CAEP-SPECIFICATION]
Cappalli, T. and A. Tulshibagwale, "OpenID Continuous Access Evaluation Profile 1.0 - draft 01", 8 June 2021, <https://openid.net/specs/openid-caep-specification-1_0-01.html>.

[RFC2119]
Bradner, S., "Key words for use in RFCs to Indicate Requirement Levels", BCP 14, RFC 2119, DOI 10.17487/RFC2119, March 1997, <https://www.rfc-editor.org/info/rfc2119>.

[RFC8174]
Leiba, B., "Ambiguity of Uppercase vs Lowercase in RFC 2119 Key Words", BCP 14, RFC 8174, DOI 10.17487/RFC8174, May 2017, <https://www.rfc-editor.org/info/rfc8174>.

[SET]
Hunt, P., Ed., Jones, M.B., Denniss, W., and M.A. Ansari, "Security Event Token (SET)", April 2018, <https://tools.ietf.org/html/draft-ietf-secevent-token-09>.

[SHARED-SIGNALS-FRAMEWORK]
Tulshibagwale, A., Cappalli, T., Scurtescu, M., Backman, A., and J. Bradley, "OpenID Shared Signals and Events Framework Specification 1.0", 8 June 2021, <https://openid.net/specs/openid-sse-framework-1_0-01.html>.


[RISC-SPECIFICATION] M. Scurtescu, A. Backman, P. Hunt, J. Bradley, S. Bounev, A. Tulshibagwale, OpenID RISC Profile Specification 1.0 - draft 03, 21 March 2023, <https://openid.github.io/sharedsignals/openid-risc-profile-specification-1_0.html#name-normative-references>



---

This draft serves as a foundational document, intended to be refined and expanded through collaboration with banking sector stakeholders, cybersecurity experts, and regulatory bodies to ensure its effectiveness and compliance with global standards.