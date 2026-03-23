# peppol

# SK new RULES

| Rule ID | BT | UBL Element / Condition | Legal basis |
| :--- | :--- | :--- | :--- |
| **Unconditional** | | (0..1 → 1..1) | |
| SK-R-001 | BT-7 | TaxPointDate | §74/1/d zák. 222/2004 |
| SK-R-002 | BT-9 | DueDate (for Invoice) | |
| SK-R-003 | BT-30 | Seller PartyLegalEntity/CompanyID | |
| SK-R-004 | BT-31 | Seller PartyTaxScheme/CompanyID (VAT) | §74/1/b |
| SK-R-005 | BT-35 | Seller PostalAddress/StreetName | §74/1/a |
| SK-R-006 | BT-37 | Seller PostalAddress/CityName | §74/1/a |
| SK-R-007 | BT-38 | Seller PostalAddress/PostalZone | §74/1/a |
| SK-R-008 | BT-48 | Buyer PartyTaxScheme/CompanyID (only for vat payer) | |
| SK-R-009 | BT-50 | Buyer PostalAddress/StreetName | §74/1/b |
| SK-R-010 | BT-52 | Buyer PostalAddress/CityName | §74/1/b |
| SK-R-011 | BT-53 | Buyer PostalAddress/PostalZone | §74/1/b |
| SK-R-012 | BT-110 | TaxTotal/TaxAmount | |
| SK-R-013 | BT-119 | TaxCategory/Percent | §74/1/i |
| SK-R-020 | BT-9 | DueDate or PaymentMeans/PaymentDueDate (for CreditNote) | |
| SK-R-021 | BT-33 | Seller PartyLegalEntity/CompanyLegalForm | |
| **Conditional** | | | |
| SK-R-014 | BT-120/121 | When tax category is E/G/O/K/AE | §74/1/j |
| SK-R-015 | BT-148 | When price AllowanceCharge exists | |
| SK-R-016 | BT-64 | When TaxRepresentativeParty exists | §74/1/a |
| SK-R-017 | BT-66 | When TaxRepresentativeParty exists | §74/1/a |
| SK-R-018 | BT-67 | When TaxRepresentativeParty exists | §74/1/a |
| SK-R-019 | BT-25 | For CreditNote type 381 | |

# Schematron

```xml
<pattern>
    <!-- SLOVAKIA -->    
    <let name="SKSupplierCountry"
      value="concat(ubl-creditnote:CreditNote/cac:AccountingSupplierParty/cac:Party/cac:PostalAddress/cac:Country/cbc:IdentificationCode, ubl-invoice:Invoice/cac:AccountingSupplierParty/cac:Party/cac:PostalAddress/cac:Country/cbc:IdentificationCode)" />    
    <rule
      context="ubl-creditnote:CreditNote[$SKSupplierCountry = 'SK'] | ubl-invoice:Invoice[$SKSupplierCountry = 'SK']">
      <assert id="SK-R-001" flag="fatal" test="exists(cbc:TaxPointDate) and not(normalize-space(cbc:TaxPointDate)='')">
          For SK suppliers tax point date (dátum zdaniteľného plnenia) [BT-7] is mandatory (§74 ods. 1 písm. d) zákona 222/2004 Z.z.).
      </assert>     
      <assert id="SK-R-003" flag="fatal" test="exists(cac:AccountingSupplierParty/cac:Party/cac:PartyLegalEntity/cbc:CompanyID) and not(normalize-space(cac:AccountingSupplierParty/cac:Party/cac:PartyLegalEntity/cbc:CompanyID)='')">
          For SK suppliers seller legal registration identifier (IČO) [BT-30] is mandatory.
      </assert>
      <assert id="SK-R-004" flag="fatal" test="exists(cac:AccountingSupplierParty/cac:Party/cac:PartyTaxScheme[cac:TaxScheme/cbc:ID = 'VAT']/cbc:CompanyID) and not(normalize-space(cac:AccountingSupplierParty/cac:Party/cac:PartyTaxScheme[cac:TaxScheme/cbc:ID = 'VAT']/cbc:CompanyID)='')">
          For SK suppliers seller VAT identifier (IČ DPH) [BT-31] is mandatory (§74 ods. 1 písm. b) zákona 222/2004 Z.z.).
      </assert>
      <assert id="SK-R-005" flag="fatal" test="exists(cac:AccountingSupplierParty/cac:Party/cac:PostalAddress/cbc:StreetName) and not(normalize-space(cac:AccountingSupplierParty/cac:Party/cac:PostalAddress/cbc:StreetName)='')">
          For SK suppliers seller address line 1 (ulica) [BT-35] is mandatory (§74 ods. 1 písm. a) zákona 222/2004 Z.z.).
      </assert>
      <assert id="SK-R-006" flag="fatal" test="exists(cac:AccountingSupplierParty/cac:Party/cac:PostalAddress/cbc:CityName) and not(normalize-space(cac:AccountingSupplierParty/cac:Party/cac:PostalAddress/cbc:CityName)='')">
          For SK suppliers seller city (mesto) [BT-37] is mandatory (§74 ods. 1 písm. a) zákona 222/2004 Z.z.).
      </assert>
      <assert id="SK-R-007" flag="fatal" test="exists(cac:AccountingSupplierParty/cac:Party/cac:PostalAddress/cbc:PostalZone) and not(normalize-space(cac:AccountingSupplierParty/cac:Party/cac:PostalAddress/cbc:PostalZone)='')">
          For SK suppliers seller post code (PSČ) [BT-38] is mandatory (§74 ods. 1 písm. a) zákona 222/2004 Z.z.).
      </assert>
      <assert id="SK-R-008" flag="warning" test="exists(cac:AccountingCustomerParty/cac:Party/cac:PartyTaxScheme/cbc:CompanyID) and not(normalize-space(cac:AccountingCustomerParty/cac:Party/cac:PartyTaxScheme/cbc:CompanyID)='')">
          For SK suppliers buyer VAT identifier (IČ DPH kupujúceho) [BT-48] is mandatory.
      </assert>
      <assert id="SK-R-009" flag="fatal" test="exists(cac:AccountingCustomerParty/cac:Party/cac:PostalAddress/cbc:StreetName) and not(normalize-space(cac:AccountingCustomerParty/cac:Party/cac:PostalAddress/cbc:StreetName)='')">
          For SK suppliers buyer address line 1 (ulica kupujúceho) [BT-50] is mandatory (§74 ods. 1 písm. b) zákona 222/2004 Z.z.).
      </assert>
      <assert id="SK-R-010" flag="fatal" test="exists(cac:AccountingCustomerParty/cac:Party/cac:PostalAddress/cbc:CityName) and not(normalize-space(cac:AccountingCustomerParty/cac:Party/cac:PostalAddress/cbc:CityName)='')">
          For SK suppliers buyer city (mesto kupujúceho) [BT-52] is mandatory (§74 ods. 1 písm. b) zákona 222/2004 Z.z.).
      </assert>
      <assert id="SK-R-011" flag="fatal" test="exists(cac:AccountingCustomerParty/cac:Party/cac:PostalAddress/cbc:PostalZone) and not(normalize-space(cac:AccountingCustomerParty/cac:Party/cac:PostalAddress/cbc:PostalZone)='')">
          For SK suppliers buyer post code (PSČ kupujúceho) [BT-53] is mandatory (§74 ods. 1 písm. b) zákona 222/2004 Z.z.).
      </assert>
      <assert id="SK-R-012" flag="fatal" test="exists(cac:TaxTotal/cbc:TaxAmount) and not(normalize-space(cac:TaxTotal/cbc:TaxAmount)='')">
          For SK suppliers invoice total VAT amount (celková suma DPH) [BT-110] is mandatory.
      </assert>
      <assert id="SK-R-013" flag="fatal" test="exists(cac:TaxTotal/cac:TaxSubtotal/cac:TaxCategory/cbc:Percent) and not(normalize-space(cac:TaxTotal/cac:TaxSubtotal/cac:TaxCategory/cbc:Percent)='')">
          For SK suppliers VAT category rate (sadzba DPH) [BT-119] is mandatory (§74 ods. 1 písm. i) zákona 222/2004 Z.z.).
      </assert>
      <assert id="SK-R-014" flag="fatal" test="not(normalize-space(cac:TaxTotal/cac:TaxSubtotal/cac:TaxCategory/cbc:ID) = 'E' 
                  or normalize-space(cac:TaxTotal/cac:TaxSubtotal/cac:TaxCategory/cbc:ID) = 'G' 
                  or normalize-space(cac:TaxTotal/cac:TaxSubtotal/cac:TaxCategory/cbc:ID) = 'O' 
                  or normalize-space(cac:TaxTotal/cac:TaxSubtotal/cac:TaxCategory/cbc:ID) = 'K' 
                  or normalize-space(cac:TaxTotal/cac:TaxSubtotal/cac:TaxCategory/cbc:ID) = 'AE') 
                  or normalize-space(cac:TaxTotal/cac:TaxSubtotal/cac:TaxCategory/cbc:TaxExemptionReasonCode) != '' 
                  or normalize-space(cac:TaxTotal/cac:TaxSubtotal/cac:TaxCategory/cbc:TaxExemptionReason) != ''">
          For SK suppliers VAT exemption reason (dôvod oslobodenia od DPH) [BT-120/BT-121] is mandatory when tax category is exempt (§74 ods. 1 písm. j) zákona 222/2004 Z.z.).
      </assert>
      <assert id="SK-R-021" flag="fatal" test="exists(cac:AccountingSupplierParty/cac:Party/cac:PartyLegalEntity/cbc:CompanyLegalForm) and not(normalize-space(cac:AccountingSupplierParty/cac:Party/cac:PartyLegalEntity/cbc:CompanyLegalForm)='')">
          For SK suppliers company legal form (register právnických osôb) [BT-33] is mandatory.
      </assert>    
    </rule>
    <rule context="ubl-invoice:Invoice[$SKSupplierCountry = 'SK']">
        <assert id="SK-R-002" flag="fatal" test="exists(cbc:DueDate) and not(normalize-space(cbc:DueDate)='')">
            For SK suppliers payment due date (dátum splatnosti) [BT-9] is mandatory.
        </assert>
    </rule>   
    <rule context="cac:InvoiceLine/cac:Price/cac:AllowanceCharge[$SKSupplierCountry = 'SK'] | cac:CreditNoteLine/cac:Price/cac:AllowanceCharge[$SupplierCountry = 'SK']">
      <assert id="SK-R-015" flag="fatal" test="exists(cbc:BaseAmount) and not(normalize-space(cbc:BaseAmount)='')">
          For SK suppliers item gross price (cena za položku brutto) [BT-148] is mandatory when a price discount is applied.
      </assert>
    </rule>
    <rule context="(ubl-invoice:Invoice[$SKSupplierCountry = 'SK'] | ubl-creditnote:CreditNote[$SKSupplierCountry = 'SK'])/cac:TaxRepresentativeParty/cac:PostalAddress">
      <assert id="SK-R-016" flag="fatal" test="exists(cbc:StreetName) and not(normalize-space(cbc:StreetName)='')">
          For SK suppliers Tax representative address line 1 (ulica daňového zástupcu) [BT-64] is mandatory when tax representative (BG-11) is present (§74 ods. 1 písm. a) zákona 222/2004 Z.z.).
      </assert>
      <assert id="SK-R-017" flag="fatal" test="exists(cbc:CityName) and not(normalize-space(cbc:CityName)='')">
          For SK suppliers Tax representative city (mesto daňového zástupcu) [BT-66] is mandatory when tax representative (BG-11) is present (§74 ods. 1 písm. a) zákona 222/2004 Z.z.).
      </assert>
      <assert id="SK-R-018" flag="fatal" test="exists(cbc:PostalZone) and not(normalize-space(cbc:PostalZone)='')">
          For SK suppliers Tax representative post code (PSČ daňového zástupcu) [BT-67] is mandatory when tax representative (BG-11) is present (§74 ods. 1 písm. a) zákona 222/2004 Z.z.).
      </assert>
    </rule>
    <rule context="ubl-creditnote:CreditNote[$SKSupplierCountry = 'SK'][cbc:CreditNoteTypeCode = '381']">
      <assert id="SK-R-019" flag="fatal" test="exists(cac:BillingReference/cac:InvoiceDocumentReference/cbc:ID) and not(normalize-space(cac:BillingReference/cac:InvoiceDocumentReference/cbc:ID)='') ">
          For SK suppliers preceding invoice reference (referencia na pôvodnú faktúru) [BT-25] is mandatory for credit notes (type 381).
      </assert>
    </rule>
    <rule context="ubl-creditnote:CreditNote[$SKSupplierCountry = 'SK']">
      <assert id="SK-R-020" flag="fatal" test="(exists(cbc:DueDate) and not(normalize-space(cbc:DueDate)='')) or (exists(cac:PaymentMeans/cbc:PaymentDueDate) and not(normalize-space(cac:PaymentMeans/cbc:PaymentDueDate)=''))">
        For SK suppliers payment due date (dátum splatnosti) [BT-9] is mandatory.
      </assert>
    </rule>  
  </pattern>
  ```
