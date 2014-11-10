## [XSD](http://www.w3schools.com/schema/default.asp)
  XML Schema Definition.

* XML Schema ia powerful than DTDs.

  - support for data types
  - **define** data facets (restrictions on data)
  - **define** data patterns
  - convert data between data types.

* use xml parser to parse the schema file.

* extensible
  - reference multiple schemas in the same document

### Introduction
#### what is XML Schema ?
 Describes the structure of an XML document.

* defines elements, attributes that can appear in a document.
* defines which elements are child elements
* defines the **order** of child elements
* defines the **number** of child elements.
* defines whether an element is empty or can include text...
* defines data types for elements and attributes.
* defines default and fixed values for ele and attr.

**Note**: XSD can even give constraint on the number of child elements. So it may be
support define constraint on source and view.


### XSD Schema

#### The schema element
the root element of every XML Schema.


    <?xml version="1.0"?>

    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
    targetNamespace="http://www.w3schools.com"
    xmlns="http://www.w3schools.com"
    elementFormDefault="qualified">
    ...
    ...
    </xs:schema>

* xmlns:xs="..." : indicates that the elements and data types used in the schema come from the "..." namespace; and elements and data types used shall prefixed with xs.

* targetNameSpace=".." : elements and attributes defined
here belongs to ".." namespace.

* xmlns="." : indicates the default namespace is "."

* elementFormDefault="qualified" : indicates that any elements used by the XML instance document which were declared in this schema must be namespace qualified.

#### Referencing a Schema in an XML Document
    <?xml version="1.0"?>

    <note xmlns="http://www.w3schools.com"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.w3schools.com note.xsd">

    <to>Tove</to>
    <from>Jani</from>
    <heading>Reminder</heading>
    <body>Don't forget me this weekend!</body>
    </note>

* xmlns="...": specifies the default namespace declaration.
* xmlns:xsi="..": XML Schema Instance namespace
* xsi:schemaLocation=".. ..": namespace and the location of the XML schema.

### Simple Element

* canot contain any other element or attributes
* can only contain text with different types: boolean, string, date, etc. or custom type.
* add facets to a data type, or to match specific pattern.

#### syntax

    <xs:element name="xxx" type="yyy" />

XML Schema built-in data types:
* xs:string
* xs:decimal
* xs:integer
* xs:boolean
* xs:date
* xs:time

#### Default and Fixed values

    <xs:element name="color" type="xs:string" default="red" />

or

    <xs:element name="color" type="xs:string" fixed="red" />

#### Attributes

attributes are declared as simple types

**simple element  cannot have attributes**

##### attribute syntax

     <xs:attribute name="xxx" type="yyy" />
##### Default and Fixed Values

   default="" , fixed=""


##### Optional and Required Attributes
* **Attributes are optional by default**

  to specify the attrubute is required, use "use" attribute. e.g use="required"

      <xs:attribute name="lang" type="xs:string" use="required" />



##### Restrictions on Content

* type matching

Restrictions on XML elements are called facets.

* Restriction on Values

    <xs:element name="age">
      <xs:simpleType>
        <xs:restriction base="xs:integer">
          <xs:minInclusive value="0"/>
          <xs:maxInclusive value="120"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:element>

 specify restrictions on the element age in the type part.

 - xs:minInclusive, xs:maxInclusive. use value="" to assign values.

* Restrictions on a Set of Values
use enumeration constraint

  <xs:element name="car">
    <xs:simpleType>
      <xs:restriction base="xs:string">
        <xs:enumeration value="Audi"/>
        <xs:enumeration value="Golf"/>
        <xs:enumeration value="BMW"/>
      </xs:restriction>
    </xs:simpleType>
  </xs:element>

 - base is used to specify the type for this attribute.
 - xs:enumeration to define the enumeration values.

Or you can define a new type called "carType":

    <xs:element name="car" type="carType"/>

    <xs:simpleType name="carType">
      <xs:restriction base="xs:string">
        <xs:enumeration value="Audi"/>
        <xs:enumeration value="Golf"/>
        <xs:enumeration value="BMW"/>
      </xs:restriction>
    </xs:simpleType>

* restrictions on a Series of Values

  **use the pattern constraint.**

     <xs:element name="letter">
       <xs:simpleType>
         <xs:restriction base="xs:string">
            <xs:pattern value="[a-z]" />
         </xs:restriction>
       </xs:simpleType>
     </xs:element>

  * xs:pattern="[A-Z][A-Z][A-Z]"
  * xs:pattern="[a-zA-Z][a-zA-Z][a-zA-Z]"
  * xs:pattern="[xyz]" : only accept one of xyz.
  * xs:pattern="[0-9][0-9][0-9][0-9][0-9]"

* Other Restrictions on a Series of Values

  * xs:pattern="(a-z)*"
  * xs:pattern="([a-z][A-Z])+"
  * xs:pattern="male|female"
  * xs:pattern="[a-zA-Z0-9]{8}"

* Restrictions on WhiteSpace Characters

    <xs:element name="address">
      <xs:simpleType>
        <xs:restriction base="xs:string">
           <xs:whiteSpace value="preserve" />
        </xs:restriction>
      </xs:simpleType>
    </xs:element>

   preserve means the XML processor WILL NOT remove any white space characters.

   * xs:whiteSpace value="replace" : will REPLACE all white space characters (line fees, tabs, spaces and carriage returns) with white spaces.

   * xs:whiteSpace value="collapse" : will REMOVE all...

* Restrictions on Length

   maxLength, minLength

      <xs:element name="password">
        <xs:simpleType>
           <xs:restriction base="xs:string">
              <xs:length value="8" />
           </xs:restriction>
        </xs:simpleType>
      </xs:element>

  another example

      <xs:element name="password">
        <xs:simpleType>
           <xs:restriction base="xs:string">
              <xs:minLength value="5">
              <xs:maxLength value="8">
           </xs:restriction>
        </xs:simpleType>
      </xs:element>

* [Restrictions for Datatypes](http://www.w3schools.com/schema/schema_facets.asp)

### XSD Complex Elements
Four kinds of complex elements:
* empty elements
* elements that contains only other elements
* element that contain only text
* **element that contain both other elements and text**

* Each of these elements may contain attributes.

##### Define complex element

     <employee>
        <firstname>Stefan</firstname>
        <lastname>Zan</lastname>
     </employee>

The XML Schema:

    <xs:element name="employee">
       <xs:complexType>
         <xs:sequence>
           <xs:element name="firstname" type="xs:string" />
           <xs:element name="firstname" type="xs:string" />
         </xs:sequence>
       </xs:complexTye>
    </xs:element>

Or you can abstract it as a datatype

     <xs:complexType name="personalinfo"
      <xs:sequence>
        <xs:element name="firstname" type="xs:string" />
        <xs:element name="firstname" type="xs:string" />
      </xs:sequence>
     </xs:complexType>

Then refer to it as a type

     <xs:element name="employee" type="personalinfo" />


* complex type can also be extend using: xs:extension


    <xs:element name="employee" type="fullpersoninfo"/>

    <xs:complexType name="personinfo">
      <xs:sequence>
        <xs:element name="firstname" type="xs:string"/>
        <xs:element name="lastname" type="xs:string"/>
      </xs:sequence>
    </xs:complexType>

    <xs:complexType name="fullpersoninfo">
      <xs:complexContent>
        <xs:extension base="personinfo">
          <xs:sequence>
            <xs:element name="address" type="xs:string"/>
            <xs:element name="city" type="xs:string"/>
            <xs:element name="country" type="xs:string"/>
          </xs:sequence>
        </xs:extension>
      </xs:complexContent>
    </xs:complexType>


  Note the line: xs:extension base="personalinfo"

  which means it is an extension of personalinfo

##### XSD Empty Elements
An empty complex element cannot have contents, only attributes.

* Note: A simple element canot contain any other elements or attributes. only contain text.

      <xs:element name="product">
         <xs:complexType>
            <xs:attribute name="productid" type="xs:positiveInteger" />
         </xs:complexType>
      </xs:element>


Or:


<xs:element name="product">
   <xs:complexType>
     <xs:complexContent>
        <xs:restriction base="xs:integer">
           <xs:attribute name="prodid" type="xs:positiveInteger">
        </xs:restriction>
     </xs:complexConent>
   </xs:complexType>
</xs:element>

**:** define a complex type ?

##### XSD Elements Only

A "element-only" complex type contains an element that contains only other elements.

    <xs:element name="person">
       <xs:complexType>
          <xs:sequence>
             <xs:element name="firstname" type="xs:string" />
             <xs:element name="lastname" type="xs:string" />
          </xs:sequence>
       </xs:complexType>
    </xs:element>
* xs:sequence tag means that the elements defined must appear in that order inside the "person" element.

##### XSD Text-only Elements

Only contain text and attributes.

add **simpleContent** around the content.

When using simpleContent, you must define an extension OR a restriction within the simpleContent element.

Example 0.


    <shoesize>35</shoesize>

is:

     <xs:element name="shoesize" type="xs:integer">

Example 1.

    <shoesize country="france">35</shoesize>
is:

     <xs:element name="shoesize">
       <xs:complexType>
          <xs:simpleContent>
             <xs:extension base="xs:integer">
                <xs:attribute name="country" type="xs:string" />
             </xs:extension>
          </xs:simpleContent>
       </xs:complexType>
     </xs:element>


评论：这个做法还是挺费解的，对shoesize这个integer的类型进行扩展！！！！
扩展为一个string。。。。无厘头



      <xs:element name="shoesize" type="shoetype"/>

      <xs:complexType name="shoetype">
        <xs:simpleContent>
          <xs:extension base="xs:integer">
            <xs:attribute name="country" type="xs:string" />
          </xs:extension>
        </xs:simpleContent>
      </xs:complexType>

这里定义一个shoetype，还感觉能说得通。

##### XSD Mixed

A mixed complex type element can contain attributes, element, and text.

Example:

     <letter>
       Dear Mr.<name>John</name>.
       Your order <orderid>1032</orderid>
       will be shipped on <shipdate>2001-07-13</shipdate>
     </letter>

The XSD:

     <xs:element name="letter">
       <xs:complexType mixed="true">
         <xs:sequence>
            <xs:element name="name" type="xs:string" />
            <xs:element name="orderid" type="xs:positiveInteger" />
            <xs:element name="shipdate" type="xs:date" />
         </xs:sequenc>
       </xs:complexType>
     </xs:element>

* to enable character data to appear between the child-elements of "letter", the mixed attribute of xs:complexType must be set to "true".



##### XSD Indicators
* order indicators
   - All: the child elements can appear in any order, and each element must occur only once.

   When using all, can set minOccurs to 0 or 1, and maxOccurs which can only be set to 1.

   - Choice: either one child element or another can occur.

        <xs:element name="person">
           <xs:complexType>
             <xs:choice>
               <xs:element name="employee" type="employee" />
               <xs:element name="member" type="member" />
             </xs:choice>
           </xs:complexType>
        </xs:element>


   - Sequence

* occurrence indicators

  The default minOccurs and maxOccurs is 1.

   - maxOccurs

          <xs:element name="child_name" type="xs:string" maxOccurs="10" />

   - minOccurs


To allow an element to appear an unlimited number of times, use the maxOccurs="unbounded" statement.

* group indicators
   - Group name
   - attributeGroup name

Detail:
   * Element Groups

          <xs:group name="groupname">
          ...
          </xs:group>

   * Attribute Groups

          <xs:attributeGroup name="groupname">
          ...
          </xs:attributeGroup>


Then it can be referred in other definitions.

    <xs:sequence>
       <xs:group ref="groupname" />
       <xs:element name="" typ="xs:string" />
    </xs:sequence>

for attribute:

     <xs:complexType>
       <xs:attributeGroup ref="groupname" />
     </xs:complexType>

##### any Element

any is used to make EXTENSIBLE documents.

[w3c school any](http://www.w3schools.com/schema/schema_complex_any.asp)

##### anyAttribute element




### XSD String Data Types

#### String Data Type

string data type can contain characters, line feeds, carriage returns, and tab characters.

##### NormalizedString Data Type

the XML processor will remove line feeds, carriage returns and tab characters.

    <xs:element name="customer" type="xs:normalizedString" />

**Just replace tables with spaces.**

##### Token Data Type
