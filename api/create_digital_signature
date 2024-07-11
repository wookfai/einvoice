======
Document Signature Creation
======

High-Level Process
The overall steps to take when preparing a single document for submission are:

Step 1: Create document XML or JSON (no signature elements yet) for a single document
Step 2: Apply transformations to the document
Step 3: Canonicalize the document and generate the document hash (digest) (property reference DocDigest)
Step 4: Sign the document digest (property reference Sig)
Step 5: Generate the certificate hash (Digest) (property reference CertDigest)
Step 6: Populate the signed properties section
Step 7: Generate Signed Properties Hash (Digest) (property reference PropsDigest)
Step 8: Populate the information in the document to create the signed document
#Detailed Steps
#Step 2: Apply transformations to the document
Make sure the source document is in the UTF-8 format.
Remove the XML version.
Remove the not required elements if any exists (UBLExtension (XPath: [local-name()=׳Invoice׳]//[local-name()=׳UBLExtensions׳]), and Signature (XPath: [local-name()=׳Invoice׳]//[local-name()=׳Signature׳]))
#Step 3: Canonicalize the document and generate the document hash (digest)
Prepare document canonical version, to be used for signing:
XML format - apply xml-c14n11 canonicalization to the document
JSON format - minify the document by removing following elements from it:
Whitespaces
Line breaks
Comments
Hash the canonicalized document invoice body using SHA-256.
Use a HEX-to-Base64 encoder to encode the hashed value and convert it to base 64.
The output will be set as the value of the property DocDigest
#Step 4: Sign the document digest
Sign the generated invoice hash with RSA-SHA256 using the signing certificate private key
The output will be set as the value of the property Sig
Encode the hashed property tag using Base64 Encoder
#Step 5: Generate the certificate hash
Hash the signing certificate using SHA-256
Encode the certificate hash using Base64 encoding
#Step 6: Populate the signed properties section
Update the document, being signed, by filling-in the following properties as per the below guidance:

FIELD	VALUE	UBL MAPPING PATH
DigestValue	Encoded certificate hashed property reference CertDigest	/Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:Object /xades:QualifyingProperties / xades:SignedProperties/xades:SignedSignatureProperties / xades:SigningCertificate /xades:Cert /xades:CertDigest /ds:DigestValue
SigningTime	Sign timestamp as current datetime in UTC	/Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:Object /xades:QualifyingProperties /xades:SignedProperties /xades:SignedSignatureProperties /xades:SigningTime
X509IssuerName	Signing certificate issuer name	/Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:Object /xades:QualifyingProperties / xades:SignedProperties /xades:SignedSignatureProperties / xades:SigningCertificate /xades:Cert /xades:IssuerSerial /ds:X509IssuerName
X509SerialNumber	Signing certificate serial number	/Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:Object /xades:QualifyingProperties / xades:SignedProperties /xades:SignedSignatureProperties / xades:SigningCertificate /xades:Cert /xades:IssuerSerial /ds:X509SerialNumber
Now the signing properties section should be ready to be hashed.

#Step 7: Generate Signed Properties Hash
Get the properties tag only using the XPath (/Invoice/ext:UBLExtensions/ext:UBLExtension/ext:ExtensionContent/sig:UBLDocumentSignatures/sac:SignatureInformation/ds:Signature/ds:Object/xades:QualifyingProperties/xades:SignedProperties)
Linearize the XML block (properties tag) and remove the spaces
Hash the property tag using SHA-256.
Encode the hashed property tag using Base64 Encoder
The value generated would be the property named PropsDigest
#Step 8: Populate the information in the document to create the signed document
Populate the properties gathered into the document as per the below table to generate the signed document.

FIELD	VALUE	UBL MAPPING PATH
SignatureValue	Digital Signature property ref Sig	/Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:SignatureValue
X509Certificate	Certificate	/Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:KeyInfo /ds:X509Data /ds:X509Certificate
DigestValue	Encoded signed Properties hash property reference PropsDigest	/Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:SignedInfo /ds:Reference[@URI=’#id-xades-signed-props’] /ds:DigestValue
DigestValue	Encoded invoice hash property reference DocDigest	/Invoice /ext:UBLExtensions /ext:UBLExtension /ext:ExtensionContent /sig:UBLDocumentSignatures /sac:SignatureInformation /ds:Signature /ds:SignedInfo /ds:Reference[@Id=’id-doc-signed-data’] /ds:DigestValue
Note: To ensure data transfer over network, potential newline symbols or spaces added removed between XML and JSON elements are not changing the signature value, the solution leverages specialized data document canonicalization approach to ensure only significant data (names and values of fields) is used as part of the signature.

Once multiple documents are prepared as per description above they all need to be added into documents array when JSON is used and into submission/documents element when XML is used. After that the submission is ready to be sent to the MyInvois System by calling Submit Document API.
