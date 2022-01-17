## Mint your own NFT project using Python

When it comes to NFT and Cryptos we often have a misconception in our mind that we need to write solidity or rust, which is somewhat true but you can still create your full-fledged project using any other language be it python or javascript, and deploy a full production grade NFT project like Crypto Punks.

In this article, we will start by creating our own NFT project named **Popular At HashNode** with the token name PAHN. This NFT project is comprised of the most popular tags at HashNode for Example Javascript, React, etc. 

Let's First start by creating our NFT design, as this post is more about minting NFT, therefore, we will just use a plain and simple design. Here is how our NFT will look like.



![JS.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641886621195/wo2gBowaW.png)

we might also need to add meta properties as seen in the open sea which is kind of a characteristic of NFT


# NFTPORT
We will be using  [NFTport.xyz](https://docs.nftport.xyz/docs)  API to create our NFT project, for coding I will be using python but you can use any language as we just need to interact with the API. Throughout the article, I will also try to cover up a few NFT and crypto jargon.

Let's start our super cool NFT project Named **Popular At HashNode**

### Signup at NFTPORT
The first step first, let signup at the NFTport  [here](https://www.nftport.xyz/sign-up)  to get the API keys

Once you have the API keys let's move to the next step in setting up the python project

### Create the smart contract

Before we start minting the NFT, we first need to create the smart contract which is nothing but the ERC721 contract also known as the NFT contract. As ethereum chain has very high fees we will be opting for a polygon chain, polygon chain is a layer 2 chain on ethereum.

Find the code below

```
import requests

url = "https://api.nftport.xyz/v0/contracts"

payload = "{\n  \"chain\": \"polygon\",\n  \"name\": \"Popular At HashNode\",\n  \"symbol\": \"PAHN\",\n  \"owner_address\": \"0xF62735d60151852dAFACfF270a926a7911b6705e\",\n  \"metadata_updatable\": false\n}"
headers = {
    'Content-Type': "application/json",
    'Authorization': "API KEY WE GENERATED ABOVE"
    }

response = requests.request("POST", url, data=payload, headers=headers)

print(response.text)
``` 
In the above code, we are 

1.  Selecting chain as **polygon**, 
2. Setting name of our contract to **Popular At Hashnode
**
3. Contract Symbol is the short symbol of the contract which can also be seen in polygonscan network
4. Setting up the owner address this is the wallet address of the owner who is minting the contract, in this case, it's my wallet address

Once you run the code above, your contract will be added to the transactions and you will be given a response like below. Make sure to save this response somewhere save as we will be needing
```
{
  "response": "OK",
  "chain": "polygon",
  "transaction_hash": "0x5162c16d209340b5c13af4428e30f76f6e567eb1a7837bd9ff800025e9d8cec4",
  "transaction_external_url": "https://polygonscan.com/tx/0x5162c16d209340b5c13af4428e30f76f6e567eb1a7837bd9ff800025e9d8cec4",
  "owner_address": "0xF62735d60151852dAFACfF270a926a7911b6705e",
  "name": "Popular At HashNode",
  "symbol": "PAHN"
}
```
Now, you can find various details related to the transaction but what we need is the contract address. To get the contract address we need to take the transaction hash and use the 
```
get_contract_address API.
``` 
Find the code below

```
import http.client

conn = http.client.HTTPSConnection("api.nftport.xyz")

headers = {
    'Content-Type': "application/json",
    'Authorization': "Your API key"
    }

conn.request("GET", "/v0/contracts/0x5162c16d209340b5c13af4428e30f76f6e567eb1a7837bd9ff800025e9d8cec4?chain=polygon", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
``` 

In the above code, you can see that I have passed the **transaction_hash** in the request.

After running the code successfully you will see the below response 


```
{
  "response": "OK",
  "chain": "polygon",
  "contract_address": "0x8E187F7b8D62f3F9f1490A3B8659ac03ca34C540",
  "transaction_hash": "0x5162c16d209340b5c13af4428e30f76f6e567eb1a7837bd9ff800025e9d8cec4",
  "error": null
}
``` 
As you can see we need the contract address, this is where we will mint our NFT

### Hooraaayyyyy It's time to mint some NFT's 🎉🎉

1. **Upload the NFT's to IPFS**

Images are quite big in size and that's the reason, we never store images directly on Blockchain as it will increase the size and won't be efficient at all.
To solve this problem, we normally upload the image to Decentralized hosting and one of the best ways to host files on a decentralized system is IPFS you can read more about it  [here](https://ipfs.io/).

Once we upload the image we take the meta URI of the uploaded image and we add that while minting our NFT to the contract that we created above

Find the code to upload the file on the IPFS

```
import requests

file = open("image.png", "rb")

response = requests.post(
    "https://api.nftport.xyz/v0/files",
    headers={"Authorization": 'API-Key-Here'},
    files={"file": file}
)
```

Once the code is successfully executed successfully, you will see the below output


```

{
  "response": "OK",
  "ipfs_url": "https://ipfs.io/ipfs/bafkreigfpjajcargicualhqfbsyhxk2e2uf2rfplvvmdrjx2coamhpkoaq",
  "file_name": "Screenshot 2022-01-11 at 1.04.01 PM.png",
  "content_type": "image/png",
  "file_size": 28770,
  "file_size_mb": 0.0274,
  "error": null
}

``` 
You can visit the above IPFS URL to see our NFT being hosted on IPFS


2. **Upload the NFT's Metadata to IPFS**
Metadata is properties associated with NFT, you should store those properties off-chain and again we will be using IPFS to store the metadata. Metadata is nothing but a JSON file with different attributes which are also visible in Opensea

You can find the Sample JSON below




```

{
  "name": "Javascript NFT",
  "description": "Top Hashnode Trending Topic",
  "file_url": "https://ipfs.io/ipfs/bafkreigfpjajcargicualhqfbsyhxk2e2uf2rfplvvmdrjx2coamhpkoaq",
  "attributes": [
    {
      "trait_type": "Background",
      "value": "Yellow"
    },
    {
      "trait_type": "Language",
      "value": "Javascript"
    }
  ]
}
``` 
In the above example, you can see **file_url** this is nothing but the URL of the file that we got in the previous step as **ipfs_url**. 
Attributes are the properties associated with the NFT it can be anything, if you are creating a game NFT character then it can be **MAX HP** or **MANA**, you can skip this step also, but in our case, we will have 2 properties 

**1. Background: Yellow** 

**2. Language: Javascript**

You can find the metadata uploading code below



```
import http.client

conn = http.client.HTTPSConnection("api.nftport.xyz")

payload = "{\n  \"name\": \"Javascript NFT\",\n  \"description\": \"Top Hashnode Trending Topic\",\n  \"file_url\": \"https://ipfs.io/ipfs/bafkreigfpjajcargicualhqfbsyhxk2e2uf2rfplvvmdrjx2coamhpkoaq\",\n  \"attributes\": [\n    {\n      \"trait_type\": \"Background\",\n      \"value\": \"Yellow\"\n    },\n    {\n      \"trait_type\": \"Language\",\n      \"value\": \"Javascript\"\n    }\n  ]\n}"

headers = {
    'Content-Type': "application/json",
    'Authorization': "<api-key>"
    }

conn.request("POST", "/v0/metadata", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
``` 

After running the above code successfully you will see the below response 


```
{
  "response": "OK",
  "metadata_uri": "ipfs://bafkreiepfwroxy47wzvllrfyu5dwecz6blkiztzmc3r3frryasdve74zii",
  "name": "Javascript NFT",
  "description": "Top Hashnode Trending Topic",
  "file_url": "https://ipfs.io/ipfs/bafkreigfpjajcargicualhqfbsyhxk2e2uf2rfplvvmdrjx2coamhpkoaq",
  "external_url": null,
  "animation_url": null,
  "custom_fields": null,
  "attributes": [
    {
      "trait_type": "Background",
      "value": "Yellow",
      "max_value": null,
      "display_type": null
    },
    {
      "trait_type": "Language",
      "value": "Javascript",
      "max_value": null,
      "display_type": null
    }
  ],
  "error": null
}
``` 
In the above response, you will see **metadata_uri** this is what holds your NFT and this URI is saved on the smart contract or NFT contract.

Most of the dirty work is done and now it's time to Mint NFT's to our contract and makes it available on opensea.


3. **Mint NFT's To Contract**

Now it's time to mint the NFT uploaded in step 2 to actual contract, let's look at the code below


```
import http.client

conn = http.client.HTTPSConnection("api.nftport.xyz")

payload = "{\n  \"chain\": \"polygon\",\n  \"contract_address\": \"0x8E187F7b8D62f3F9f1490A3B8659ac03ca34C540\",\n  \"metadata_uri\": \"ipfs://bafkreiepfwroxy47wzvllrfyu5dwecz6blkiztzmc3r3frryasdve74zii\",\n  \"mint_to_address\": \"0xF62735d60151852dAFACfF270a926a7911b6705e\"\n}"

headers = {
    'Content-Type': "application/json",
    'Authorization': "<api-key>"
    }

conn.request("POST", "/v0/mints/customizable", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
``` 
You can find the above payload definition below 


```
{
  "chain": "polygon",
  "contract_address": "The contract address which you got from step 1",
  "metadata_uri": "Metadata URI that you got from the response of uploading metadata to IPFS",
  "mint_to_address": "Account address where the NFT will be sent. For example, your Metamask wallet address if you wish to send it to yourself"
}
``` 

After running the above code successfully you will see the below response 
``` 
{
  "response": "OK",
  "chain": "polygon",
  "contract_address": "0x8E187F7b8D62f3F9f1490A3B8659ac03ca34C540",
  "transaction_hash": "0x4aad15cab3cda4a6a3175165b33ef5178bf0468e76f4887fe7faed4876c523cc",
  "transaction_external_url": "https://polygonscan.com/tx/0x4aad15cab3cda4a6a3175165b33ef5178bf0468e76f4887fe7faed4876c523cc",
  "metadata_uri": "ipfs://bafkreiepfwroxy47wzvllrfyu5dwecz6blkiztzmc3r3frryasdve74zii",
  "mint_to_address": "0xF62735d60151852dAFACfF270a926a7911b6705e",
  "error": null
}
``` 
Congrats we have created our very own NFT with customized Metadata

You can have a look at the following links to confirm the NFT

 [Polygon Scan URL](https://polygonscan.com/tx/0x4aad15cab3cda4a6a3175165b33ef5178bf0468e76f4887fe7faed4876c523cc) 

[Opensea URL](https://opensea.io/assets/matic/0x8e187f7b8d62f3f9f1490a3b8659ac03ca34c540/200752585380054219878)

Below is the screenshot of the polygon scan URL

![Screenshot 2022-01-11 at 3.42.25 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641895971063/xe0Bzf99l.png)

You can see the total supply as 1 PAHN, if you mint more you will see the number accordingly. Also, have a look at the name and token of our NFT this is exactly what we have set while creating the NFT contract

Below is the screenshot of the opensea URL, Also notice the properties tab as well

![Screenshot 2022-01-11 at 3.46.34 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641896231007/ay-TKj6bA.png)

So, I hope you like this post about NFT's, do share and subscribe.
You can find more such content on my Twitter handle  [here](https://twitter.com/chopcoding) 