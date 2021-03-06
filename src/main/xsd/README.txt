INSTRUCTIONS FOR THE AMP WSDL/XSD FILES:

The AMP WSDL/XSD files need to be copied directly from the DataPower device
"store:/" filesystem to this directory:
   - app-mgmt-protocol.wsdl
   - app-mgmt-protocol.xsd
   - app-mgmt-protocol-v2.wsdl
   - app-mgmt-protocol-v2.xsd
   
Normally these DataPower wsdl/xsd files would be fed to the xmlbeans Ant
task for generating the jar that maps the AMP calls into POJOs. However,
these DataPower wsdl/xsd files have a behavior that we don't want in
xmlbeans: enforcement of the values for DeviceType and FeatureList. This
means that if a new DeviceType or a new FeatureList is introduced, that
would require a change to the XSD and a corresponding change to the AMP jar.
This is because the AMP jar generated by xmlbeans will do schema validation
of incoming messages.

So what we want to do is remove the enums of DeviceType and FeatureList
from the xsd, so that any valid string will be accepted. To do that,
copy "app-mgmt-protocol.xsd" to "app-mgmt-protocol-xmlbeans.xsd", then
remove the enums in the latter file. Also copy "app-mgmt-protocol.wsdl"
to "app-mgmt-protocol-xmlbeans.wsdl" and change the schema "include" to
pull in the modified xsd "app-mgmt-protocol-xmlbeans.xsd".

The Ant script will verify that the modified files are newer than the
original files from the device. The Ant script will fail if they are older.

The modified xsd/wsdl files are fed to the xmlbeans task instead of the
original xsd/wsdl files. This will allow new DeviceTypes and Feature items
to be added to the device without a change to the IBM Appliance 
Management Toolkit.

SCM ID: $Id: README.txt,v 1.3 2010/08/19 13:18:24 burket Exp $