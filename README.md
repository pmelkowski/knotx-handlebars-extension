# knotx-handlebars-extension
Custom Knot.x [Handlebars](http://handlebarsjs.com/) Helpers. 

To build run:
* `mvn clean package`

To use extensions simply add `knotx-handlebars-extension-x.y.z.jar` to classpath together with Knot.x instance.

## Helpers

- [ElementHelper](#elementhelper)
- [Conditions checks](#conditions-checks)
  - [AND](#and)
  - [OR](#or)
- [Math helpers](#math-helpers)
  - [EqualHelper](#equalhelper)
  - [GreaterEqualHelper](#greaterequalhelper)
  - [GreaterThanHelper](#greaterthanhelper)
  - [LessEqualHelper](#lessequalhelper)
  - [LessThanHelper](#lessthanhelper)
- [Regex helpers](#regex-helpers)
  - [EachSplitHelper](#eachsplithelper)
  - [MatchHelper](#matchhelper)
- [Uri helpers](#uri-helpers)
  - [DecodeHelper](#decodehelper)

### ElementHelper
Get the n-th element from a json array.

**Usage**
```
{{element array n}}
```

**Example**
```
{
  locales: ["en_us", "pl_pl", "en_uk"]
}
```
```
{{element locales 2}}
```
Output: `en_uk`


### Conditions checks
#### AND
Checks if all values are not 'falsy' (if it is false, null, 0 or empty list/array)

**Usage**
```
{{#and value1 value2 ... }}
  all values not empty
{{else}}
  some or all values empty
{{/and}}
```

**Example**
```
{
  "image": {
    "imageLink": "/assets/tv-set-1323122.png",
    "title": "Super TV Set with remote",
    "id": "1323122",
    "products": ["Super TV Set", "Remote"]
  }
}
```
```
{{#and image.id image.products}}
  {{#each image.products}}
     <span>{{this}}</span> 
  {{/each}}
{{else}}
  No product for image {{image.title}}
{{/and}}

```
Output: 
```
<span>Super TV Set</span>
<span>Remote</span>
```

#### OR
Checks if any value is not 'falsy' (if it is `false`, `null`, `0` or empty list/array).

**Usage**
```
{{#or value1 value2 ... }}
  some value not empty
{{else}}
  all values empty
{{/or}}
```

**Example**
```
{
  "product": {
    "imageLink": "/assets/tv-set-1323122.png",
    "name": "Super TV",
    "technicalDescription": "OLED, 4K, WiFi",
    "producerDescription": "",
    "parts": ["Super TV Set", "Remote"]
  }
}
```
```
{{#or product.technicalDescription product.producerDescription}}
     <span>{{product.technicalDescription}}</span> 
     <span>{{product.producerDescription}}</span> 
{{else}}
  This product has no description.
{{/or}}

```
Output: 
```
<span>OLED, 4K, WiFi</span>
<span></span>
```

### Math helpers
#### EqualHelper
Checks if `value1` and `value2` are equal.

**Usage**
```
{{#eq value1 value2}}
  equal
{{else}}
  not equal
{{/eq}}
```
**Examples**
```
{
  "someIntA": 1,
  "someIntB": 5,
  "someDoubleA": 5.14,
  "someDoubleB": 3.14
}
```
```
{{#eq someIntA someIntB}}
  equal
{{else}}
  not equal
{{/eq}}
```
Output: `not equal`

#### GreaterEqualHelper
Checks if `value1` is greater or equal to `value2`.

**Usage**
```
{{#ge value1 value2}}
  greater or equal
{{else}}
  less
{{/ge}}
```
**Examples**
```
{
  "someIntA": 1,
  "someIntB": 5,
  "someDoubleA": 5.14,
  "someDoubleB": 3.14
}
```
```
{{#ge someIntA someIntB}}
  greater or equal
{{else}}
  less
{{/ge}}
```
Output: `less`
```
{{#ge someDoubleA someDoubleB}}
  greater or equal
{{else}}
  less
{{/ge}}
```
Output: `greater or equal`

#### GreaterThanHelper
Checks if `value1` is greater than `value2`.
**Usage**
```
{{#gt value1 value2}}
  greater 
{{else}}
  less or equal
{{/gt}}
```
**Examples**
```
{
  "someIntA": 1,
  "someIntB": 5,
  "someIntC": 5
}
```
```
{{#gt someIntB someIntC}}
  greater 
{{else}}
  less or equal
{{/gt}}
```
Output: `less or equal`

#### LessEqualHelper
Checks if `value1` is smaller or equal to `value2`.
**Usage**
```
 {{#le value1 value2}}
         smaller or equal
 {{else}}
         greater
 {{/le}}
```
**Examples**
```
{
  "someIntA": 1,
  "someIntB": 5
}
```
```
{{#le someIntA someIntB}}
  less or equal 
{{else}}
  greater
{{/le}}
```
Output: `less or equal`

#### LessThanHelper
Checks if `value1` is smaller than `value2`.

**Usage**
```
 {{#lt value1 value2}}
         less 
 {{else}}
         greater or equal
 {{/lt}}
```
**Examples**
```
{
  "someIntA": 1,
  "someIntB": 5
}
```
```
{{#lt someIntA someIntB}}
  less 
{{else}}
  greater or equal
{{/lt}}
```
Output: `less`

### Regex helpers
#### EachSplitHelper
Splits a value using regex delimiter, and iterates over the result.

**Usage**
```
   {{each-split value delimiter_pattern}}
```

**Examples**
```
{
  "someString":"value1,value2,value3"
}
```
```
{{#each-split someString "\,"}}
  <td>{{this}}</td>
{{/each-split}}
```

Output:
```
  <td>value1</td>
  <td>value2</td>
  <td>value3</td>
```

#### MatchHelper
Finds a matching string using [`Matcher.find` method](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html#find-int-).

**Usage**
```
   {{match value pattern [index] [group]}}
```

**Examples**
```
{
  "someString":"value1,value2,value3"
}
```
```
{{#match someString "[a-z]+[0-9]"}}
```
Output: `value1`

```
{{#match someString "[a-z]+[0-9]" 2}}
```
Output: `value3`

```
{{#match someString "[a-z]+([0-9])" 1 1}}
```
Output: `2`

### Uri helpers
#### DecodeHelper
Decodes given value as in [`URLDecoder.decode` method](https://docs.oracle.com/javase/8/docs/api/java/net/URLDecoder.html#decode-java.lang.String-java.lang.String-) 
with `UTF-8` encoding.

**Usage**
```
   {{decode_uri value}}
```

**Examples**
```
{
  "imageData": {
    "pageUrl":"/images/?image&#x3D;Some-Image-Name&amp;id&#x3D;xyz&#x3D;&#x3D"
  }
}
```
```
<link rel="canonical" href="{{decode_uri imageData.pageUrl}}" />
```

Output: `<link rel="canonical" href="/images/?image=Some-Image-Name&id=xyz%3D%3D">`

#### EncodeHelper
Encodes given value as in [`URLEncoder.encode` method](https://docs.oracle.com/javase/8/docs/api/java/net/URLEncoder.html#encode-java.lang.String-java.lang.String-) 
with `UTF-8` encoding.

**Usage**
```
   {{encode_uri value}}
```

**Examples**
```
{
  "field": "ex@mple string"
}
```
```
{{encode_uri field}}
```

Output: `ex%40mple+string`

### String helpers
#### StringEqualsHelper
Checks if current value and given parameter are equal as Strings.

**Usage**
```
{{#string_equals value "parameter"}}
  equal
{{else}}
  not equal
{{/string_equals}}
```
**Examples**
```
response : {
  "statusCode": "302"
}
```
```
{{#string_equals response.statusCode "200"}}
  equal
{{else}}
  not equal
{{/string_equals}}
```
Output: `not equal`
