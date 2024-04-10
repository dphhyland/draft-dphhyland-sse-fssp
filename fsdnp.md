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

## n. Complex Subject Members

financial_account

    OPTIONAL. A subject identifier for the financial account. 


## 3. Event Types

FSDNP is designed to facilitate the exchange of standardized event messages related to potential or detected fraud and scams among participants, enabling rapid and coordinated response mechanisms.

The bade uri for the FSDNP is https://schemas.openid.net/secevent/fsdnp/event-type.

This specification defines the following event types:


- **Unusual Account Activities**

``` json

   {
     "iss": "https://idp.example.com/3456790/",
     "jti": "756E69717565206964656E746966696572",
     "iat": 1508184845,
     "aud": "https://sp.example2.net/caep",
     "events": {
       "https://schemas.openid.net/secevent/fsdnp/event-type/unsual-activities": {
         "financial_account": {
           "format": "iban",
           "IBAN": "DE89370400440532013000"
         },
       }
     }
   }

```



- New Device Registration -> Device compliance change. 


- Failed Multi-Factor Authentication Attempts 


- **Phishing Attempts Detected**

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
        "iss": "https://www.westpac.com.au/",
        "sub": "7375626A656374",
      }
    }
  }
}

```


- High-Risk Transactions



- Account Changes -> Same as Identifer change?


- Suspicious Money Transfers


``` json

   {
     "iss": "https://idp.example.com/3456790/",
     "jti": "756E69717565206964656E746966696572",
     "iat": 1508184845,
     "aud": "https://sp.example2.net/caep",
     "events": {
       "https://schemas.openid.net/secevent/fsdnp/event-type/unsual-activities": {
         "financial_account": {
           "format": "iban",
           "IBAN": "DE89370400440532013000"
         },
       }
     }
   }
```

- Card Not Present (CNP) Fraud Alerts


- Identity Verification Failures

- SIM Swap Alerts

- Regulatory Watchlist Hits




### 4 Data Protection and Privacy

Participants MUST ensure that all FSEMs comply with applicable data protection and privacy laws. Measures such as pseudonymization of customer identifiers and encryption of event messages SHALL be implemented.

## 5. Operational Requirements

Participants in the FSDNP SHALL:
- Establish secure communication channels for the exchange of FSEMs.
- Implement mechanisms to verify the authenticity of FSEMs received from other participants.
- Respond to FSEMs in accordance with their internal fraud management and customer protection policies.

## 6. Security Considerations

Participants MUST implement appropriate security measures to protect the integrity and confidentiality of FSEMs and to prevent unauthorized access to sensitive information.

## 67. Compliance and Reporting

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