# Setting up Azure B2C Custom Policies

## Overview
This repository contains custom policies for Azure B2C. The custom policies are based on the starter pack provided by Microsoft. 

## Prerequisites
- Azure Subscription
- Azure B2C Tenant. 
  Steps to create a B2C Tenant are given [here](https://learn.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-tenant)


## Configuration Steps

### Register a new Application with the B2C Tenant
  1. Follow the steps in the link [here](https://learn.microsoft.com/en-us/azure/active-directory-b2c/tutorial-register-applications) to register a new application with the B2C tenant.
  2. Add the **Redirect URI** pointing to the appropriate application URL.
  3. Under the *Authentication* link of the registered application, enable the *Allow public client flows* slider. This will configure this application as a Public Client so that client secerts are not needed during authentication.

### Follow the below steps in sequence in the Azure Portal

   1. [Create the JWT Signing Key](https://learn.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-user-flows?pivots=b2c-custom-policy#create-the-signing-key)
   2. [Create the Token Encryption Key](https://learn.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-user-flows?pivots=b2c-custom-policy#create-the-encryption-key) 
   3. Create the *certificate* and *private key* for signing the SAML request from B2C to Azure Entra Id. Link [here](https://learn.microsoft.com/en-us/azure/active-directory-b2c/identity-provider-generic-saml?tabs=windows&pivots=b2c-custom-policy#create-a-policy-key)
   4. In all of the policy files, replace *your tenant* with the name of your Azure AD B2C tenant.
    For example, if the name of your B2C tenant is contosotenant, all instances of *yourtenant.onmicrosoft.com* become *contosotenant.onmicrosoft.com*.
   5. In *TrustFrameworkBase.xml*, the entries added under the *<!-AIOPS.D :: Custom ClaimType Definitions*****START************* -->* are the custom claim attributes that are populated in the JWT claim.
   6. In *TrustFrameworkExtensions.xml*,the **Deloitte SAML** *ClaimsProvider* element, has the SAML configurations needed for the Deloitte Entra ID. This configuration can be re-purposed for any SAML based federation.
   7. Update  *PartnerEntity* with the IDP SAML metadata document path and *StorageReferenceId* of the **CryptographicKeys** section with the certificate created in step 3.
   8. Update the *ServiceUrl* of **REST-EnrichTokenWithRoles** TechnicalProfile with the correct REST API endpoint, that will enrich the token claims with the user groups.
   9. In *SignUpOrSignin.xml*, update the *SessionExpiryInSeconds* to a value that suits your requirement.
   10. [Upload the policy files to your Azure AD B2C tenant](https://learn.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-user-flows?pivots=b2c-custom-policy#upload-the-policies)
 
It might take more than 10 mins at times, for the policies to be refreshed.
   
   - 