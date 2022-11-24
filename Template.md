# Template

## What is template?

The most efficient way to save document with in blockchain, we belive to use template and save document data in json format, so we can reuse template if we want to save another document.

We use HTML for template.

CSS and javascript are allowed.

Note:

- Animations are not recommeded because HTML will be static page.
- Event thought Javascript is allowed, javascript could makes no difference, because at the end, HTML will be converted to PDF file anyway.

### 1. HTML

Why HTML?\
Because html is very easy to create, modify, filled with data, and convert to PDF, basically very easy to manipulate.

### 2. Placeholder

Placeholder is formatted text or code, placeholder indicate what we want to replace at the template, we can define placeholder with following format.

`{{.placeholder}}`

We use json to fill the placeholder, the key in the placeholder correspond with the placeholder name, for example above the placeholder name is 'placeholder'.

Here following example for the HTML with placeholders and it's correspond data.

```HTML
<div class="container">
    <div class="university">Blockchain University</div>
    <div id="ijazah">IJAZAH</div>
    <div class="assignment">Menyatakan bahwa</div>
    <div class="name">{{.name}}</div>
    <div class="reason">
        lahir pada tanggal <em>{{.birthDate}}</em><br>
        diterima pada tahun Akedemik <em>{{.startYear}}/{{.endYear}}</em>
    </div>
    <div class="reason">
        telah meyelesaikan studi dan memenuhi segala syarat pendidikan kesarjanaan pada Program Studi<br>
        <em>{{.study}}</em><br>
        pada tanggal <em>{{.studyDate}}</em>
    </div>
    <div class="reason">
        Oleh sebab itu, kepadanya diberikan gelar<br>
        <em>{{.degree}}</em><br>
        besarta segala hak dan kewajiban yang melekat pada gelar tersebut.
    </div>
    <div class="reason">Diberikan di {{.city}}, tanggal <em>{{.certDate}}</em></div>
    <div id="photoProfile"><img style="width:90px;height:100px;" src="../images/template_profile.jpeg"></div>
    <div id="signatures" class="reason">
        <table>
            <tr>
                <td>Dekan</td>
                <td><div style="width: 100px;"></div></td>
                <td>Rektor</td>
            </tr>
            <tr>
                <td>{{.faculty}}</td>
                <td><div style="width: 100px;"></div></td>
                <td></td>
            </tr>
            <tr>
                <td>
                    <div id="signature:dekan" style="float: left; text-align: center;" qr_config="size=100px">{{.faculty}}</div>
                    <img class="signatureQrCode" src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/d0/QR_code_for_mobile_English_Wikipedia.svg/1024px-QR_code_for_mobile_English_Wikipedia.svg.png">
                </td>
                <td><div style="width: 100px;"></div></td>
                <td>
                    <img class="signatureQrCode" src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/d0/QR_code_for_mobile_English_Wikipedia.svg/1024px-QR_code_for_mobile_English_Wikipedia.svg.png">
                </td>
            </tr>
            <tr>
                <td>{{.dekanName}}</td>
                <td><div style="width: 100px;"></div></td>
                <td>{{.rektorName}}</td>
            </tr>
        </table>
    </div>
    <div>
        <table width="100%">
            <tr>
                <td>
                    <div id="kredensialku" style="float: left; text-align: center;"></div>
                </td>
                <td>
                    <div>
                        <div style="float: right; vertical-align: middle;">
                            <img style="height:100px; vertical-align: middle;" src="../images/kredensialku_chainsmart_logo.jpg"/>
                        </div>
                    </div>
                </td>
            </tr>
        </table>
    </div>
 </div>
```

```json
{
    "name": "Riyanto",
    "birthDate": "30 April 1993",
    "startYear": "2012",
    "endYear": "2017",
    "study": "Hyperledger Fabric Edition 1",
    "studyDate": "17 August 2017",
    "degree": "Sarjana Komputer(S.Kom)",
    "city": "Jakarta",
    "certDate": "9 September 2017",
    "faculty": "Blockchain Faculty",
    "dekanName": "Prof. Dr. Ir. Muhammad Anis, M.Met.",
    "rektorName": "Prof. Ari Kuncoro, SE, MA, Ph.D."
}
```

### 3. Document ID

Document id is a string which make document unique, document id usually generated by document publisher, howerer API will make document id if not provided.

Document id usually will present in the template.

![Graduation Paper](https://github.com/SolusiDataPintar/kredensialku/blob/dev/images/contoh_ijazah.jpg?raw=true)

We can see above example graduation paper has Document ID as "NOMOR" on the top left of the document.

### 4. Document Code

Document code is 8 digits alphanumeric, document code has two pars, first 3 digit indicate document publisher, the last 5 digits indicate document.

Document code is unique.

Api will generate document code randomly.

### 5. QR Code

Document code will be converted into QR Code image, and will be redered on the document itself.

we need a marker or a tag to render the document in the template.\
It will also give us information where to render the document.

We use div as the marker, with following example.

```HTML
<div id="kredensialku" style="float: left; text-align: center;"></div>
```

Note the tag div with id "kredensialku".\
The qr code will be rendered to be image with base64 with following example.

```HTML
 <div id="kredensialku" style="float: left; text-align: center;"><img style="width:100px;height:100px;" src="data:image/jpeg;base64,..."/><div>ABCDEFGH</div><div>Verifikasi dapat dilakukan di kredensialku.id</div></div>
```

With Example above, we can see qr code rendered into three parts, they will be rendered orderly from top to bottom.

- QR Code
- Document Code
- Text 'Verifikasi dapat dilakukan di kredensialku.id'

### 6. QR Code configuration

We can separate the qr code into parts with different div tags if we want to customize the order, or if we want to rendered it on different places.

- QR Code will have id "kredensialku_qr"
- Document code will have id "kredensialku_code"
- Text 'Verifikasi dapat dilakukan di kredensialku.id' will have id "kredensialku_text"

We can also customize the qr code image with attribute "qr_config" with following example.

```HTML
<div id="kredensialku" style="float: left; text-align: center;" qr_config="size=100px id=cssstyleidqrcode class=cssstyleclassqrcode style='width:500px; height:100px;'"></div>
```

configuration above will produce following HTML code

```HTML
<div id="kredensialku" style="float: left; text-align: center;" qr_config="size=100px id=cssstyleidqrcode class=cssstyleclassqrcode style='width:500px; height:100px;'">

<img style="width:500px; height:100px;" id="cssstyleidqrcode" class="cssstyleclassqrcode" src="data:image/jpeg;base64,...base64qrcodeimage...">/>

</div>
```

### 7. [Example](https://github.com/SolusiDataPintar/kredensialku/blob/dev/examples/studycertificate.html)