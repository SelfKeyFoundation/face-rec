# idv-ocr-api
Selfkey Identity Verification and OCR Passport Extration API

## Endpoints
1) Health: a health check at `/v1/health` should return `200` [GET].

2) Convert: a image (file upload) to base64 converter at `/v1/convert` [POST].

Example:
`http://localhost:1337/v1/convert`

Data:
```
curl -X POST \
  http://localhost:1337/v1/convert \
  -H 'cache-control: no-cache' \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
  -H 'postman-token: ef66c4a5-e444-aecc-56d4-2337e3fda2d7' \
  -F image=@selfie.jpg \
  -F name=selfie.jpg
```
Response:
```
{
  "result": "<BASE_64_ENCODED_STRING>"
}
```

3) Identity: a identity check at `/api/identity` [POST].

The threshold should be supplied as a query parameter.

Provide `application/json` as the `Content-Type` header.

This endpoint accepts 2 images, tries to find a face in each image, and
based on the similarity threshold supplied will determine if the faces in
the images are the same.

`POST`ed data should be a json dictionary with 2 entries. The keys provided
will be used in the response to indicate if a face was found in the image,
and the dictionary values should be the images as base64 encoded strings.

Example:  
`http://localhost:1337/v1/identity?threshold=0.4199`  

Data:  
```
{
  "document": "<BASE_64_ENCODED_STRING>",
  "selfie": "<BASE_64_ENCODED_STRING>"
}
```
Response:
```
{
  "document": [
    {
      "_x": 304.84349513053894,
      "_y": 368.51878102620446,
      "_width": 188.6308422088623,
      "_height": 260.4338713751899,
      "_text": "person 1"
    }
  ],
  "selfie": [
    {
      "_x": 154.23945009708405,
      "_y": 230.1652708702481,
      "_width": 499.0739983320236,
      "_height": 604.8607555271425,
      "_text": "person 1 (0.4)"
    }
  ],
  "identity": [
    {
      "match": {
        "_label": "person 1",
        "_distance": 0.4057449734950477
      },
      "box": {
        "_x": 154.23945009708405,
        "_y": 230.1652708702481,
        "_width": 499.0739983320236,
        "_height": 604.8607555271425
      }
    }
  ]
}
```

4) OCR: passport text extraction at `/api/ocr` [POST].

Upload a passport image (jpg or png) as the `body` content.

Example:  
`http://localhost:1337/v1/ocr`  

Data:  
```
{
  "passport": jpg or png image
}
```
Response:
```
{
  "response": "LR Assinatura do titular / ighature du titulsire Lol = Bearer's signatuire / Fiuma: del titular R N = & K e 1% 13 = 2 Este passaporte deve serassinado pelo fiuler, !::’ 2o == salvo em caso de incapacidade. i = > v‘ _ Ce posseport dott 2ire saré o e titukare, : -l . sauf en cas ofi INCAPAGIHE, 3 o< u_ B This passport must be signed, e T —— exazpt where the bearer is inable to do s 5 - e Este pasaporte debe sexfirmade porel tukr, -« s siver 5 salvo en caso de inéapacidad W A" ""iﬁ""%?f"“ FEDERATIVA DO BRASIL e : PASSAPORTE P BRA FLat1S73 | PASSPORT § .,.! m’ T W.UGNQ\‘M»OURA VESGEIRL T vq,, S5 EDUARDO X i NACIONALIDADE / NATIONALITY o BRASILEIRO(A) - B DATADE NASCIMENTO / DATE OF BIRTH. DENTIDADE N * / PERSONAL Ne. R 29/Decl1984 ol — SEXO /86X % NATURALIDADE 7 PLACE OF BIRTH v N : . ™ RECIFE/PE i = T DATADE EXPEDICAQ / DATE OF ISSUE AUTORDADE { AUTHORTY"
}
```

## Development
### Prerequisites
- Sails JS (https://sailsjs.com/)
- Face API JS (https://github.com/justadudewhohacks/face-api.js)
- TensorFlow JS (https://github.com/tensorflow/tfjs) 
- Tesseract JS (https://tesseract.projectnaptha.com/)

### Build
`npm install` will build and install required libraries.

### Run
`node app.js` will start the api exposed on port `1337`.

## Testing
### Benchmarking and evaluating
- Extended Trained Model
- Tokensale Dataset
- 39,860 different persons
- 72,225 images
- Built a passport specialist classifier
- Strategy
- Classifier combination

- Evaluation
10 Fold Cross Validation
Final Accuracy: 96.57%

## Status Report
Please check [docs folder](docs).

## Deploy
- Google Cloud (https://console.cloud.google.com/compute/instancesDetail/zones/us-east1-b/instances/face-rec-api-dev?project=selfkey2)

## Contributing
See the [contributing notes](CONTRIBUTING.md).  
