# Agresso (ERP 7) Supplier Portal Configuration
Detailed configuration steps used to integrate the Supplier Portal with our instance of Agresso.

Note: throughout the document I will be referencing the term ERP7 instead of Agresso.  Agresso is the old name for the ERP System that long term users of the application will be familiar with.

## Pre-Requisites
- Test instance of Agresso ERP 7.12 or later
- ERP7 API's set up and configured
- Azure APIM set up and configured with an API to access
- Access to a test instance of the Supplier Portal (ERP Portal)

## Set up notes
The Supplier Portal needs to be able to access the Open API specifications for the instance that we are configuring.  This means that the Open API Spec for the instance of ERP7 needs to be exposed via the Azure APIM.

In testing it was found that the ERP7 Open API specification file was too large to upload as it was over the 4MB limit.  So we tried to host it on a site that was accessible to the APIM, but the APIM appeared to just hang and either failed or nothing happened.

Cutting the file back in size and uploading had mixed results.

In the end we created a separate Open API specification file that included all the relevant api's that the Supplier Portal would require.  This appears to work.

## Configuration
The Open API specification file can be found in our Github repo under agresso/SupplierPortal/Development/APIM/OpenAPI

### ERP7 Configuration
In order to access the ERP7 api's an ERP7 service user needs to be set up within ERP7.  To set this up follow the steps below:

