# Get Started

## Run W3bstream Locally

The W3bstream [GitHub repository](https://github.com/iotexproject/w3bstream) includes a developer-friendly setup to facilitate DePIN builders building W3bstream provers for their verifiable off-chain computing.&#x20;

The setup includes the following services:

* A reference implementation for a DePIN data sequencer
* A Postgres DB as the Data Availability service
* A W3bstream coordinator and a prover as a single-node W3bstream configuration

<figure><img src="../../../../.gitbook/assets/image (8) (1).png" alt=""><figcaption><p>Tech Stack for W3bstream Development</p></figcaption></figure>

## Download demo provers

Let's start by first downloading some ready-made W3bstream project files that implement a basic "range prover" using different ZK technologies:

```sh
mkdir w3bstream && cd w3bstream
mkdir -p test/project

# Fetch the latest W3bstream release number
export RELEASE=$(curl --silent "https://api.github.com/repos/iotexproject/w3bstream/releases/latest" | jq -r .tag_name)

# Download the Risc Zero demo prover
curl -L -o test/project/10000 https://raw.githubusercontent.com/iotexproject/w3bstream/$RELEASE/test/project/10000
# Download the Halo2 demo prover
curl -L -o test/project/10001 https://raw.githubusercontent.com/iotexproject/w3bstream/$RELEASE/test/project/10001
# Download the zkWASM demo prover
curl -L -o test/project/10002 https://raw.githubusercontent.com/iotexproject/w3bstream/$RELEASE/test/project/10002
```

<details>

<summary><strong>Learn more about the examples</strong></summary>



Let's quickly describe the demo projects:

#### Risc0 "Range Prover"

Proves that a certain integer is in a certain range.

[→ Source code](https://github.com/iotexproject/w3bstream/tree/develop/examples/risc0-circuit)&#x20;

* `private_input: string` The value we want to prove is within the range, e.g. "7"
* `public_input: string` Comma separated range endpoints, e.g. "3,9"

#### Halo2 "Proof of Multiplication"

Given two input $$a$$ and $$b$$, compute the result $$4*a^2*b^2$$ and provide the proof.

[→ Source code ](https://github.com/iotexproject/w3bstream/tree/develop/examples/halo2-circuit)

* `private_a: integer` The first value
* `private_b: integer` The second value

#### zkWASM "Proof of Addition"

Given three input $$a, b$$ and $$c$$, compute the result $$a+b$$ and provide the proof that $$a+b=c$$&#x20;

[→ Source code ](https://github.com/iotexproject/w3bstream/tree/develop/examples/zkwasm-circuit)

**Inputs:**&#x20;

* `private_input: integer[]` The first value
* `public_input: integer[]` The second value

</details>

With the demo projects in place, we can proceed by fetching the docker-compose.yaml file and running W3bstream locally:

## Start the W3bstream Dev Setup &#x20;

```sh
# Fetch the latest docker-compose.yaml
curl https://raw.githubusercontent.com/iotexproject/w3bstream/$RELEASE/docker-compose.yaml > docker-compose.yaml

# Run the configuration
docker compose up -d 

# Log the sequencer, coordinator, and prover
docker compose logs -f sequencer coordinator prover
```

## Test the setup

Now that we have a basic DePIN stack running, including a data sequencer, a data availability service, and a minimal W3bstream network, let's test the setup by sending a data message targeting one of the three demo projects.

In a new terminal, type the following command to send a data message to the sequencer targeting one of the example projects:

{% tabs %}
{% tab title="Risc0 Prover" %}
<pre><code><strong>curl -X POST \
</strong>  -H "Content-Type: application/json" \
  -d '{
    "projectID": 10000,
    "projectVersion": "0.1",
    "data": "{\"private_input\":\"14\", \"public_input\":\"3,34\", \"receipt_type\":\"Snark\"}"
  }' \
  localhost:9000/message
</code></pre>

After some time, the log in the first terminal should look like similar to the one below:

```
w3bstream-sequencer    | 200 |   42.968375ms |    192.168.65.1 | POST     "/message"
w3bstream-coordinator  | msg="dispatched a task" project_id=10000 task_id=27
w3bstream-prover       | msg="get a new task" project_id=10000 task_id=27
w3bstream-prover       | msg="acquire vm instance success" vm_type=risc0
w3bstream-coordinator  | msg=stdout proof="{\"Snark\":{\"snark\":{\"a\":[[13,51,248,201,29,8,75,18,133,175,224,43,153,182,18,76,116,108,72,18,95,83,150,181,238,149,60,82,162,242,202,144],[12,175,37,4,26,25,67,209,3,240,63,121,58,84,186,151,17,115,150,37,124,105,91,165,42,121,183,217,131,16,203,210]],\"b\":[[[34,33,95,242,250,124,78,175,229,146,174,12,184,28,240,133,228,90,88,209,196,2,235,47,121,225,159,104,237,152,210,182],[29,175,79,126,19,96,201,151,4,181,4,238,150,139,165,172,133,237,156,92,223,86,25,143,46,208,51,188,171,20,76,62]],[[12,87,52,171,230,179,182,56,194,172,191,150,227,40,164,204,200,238,82,222,83,112,138,149,134,56,14,178,165,237,81,137],[44,149,186,174,109,232,219,73,234,193,62,238,165,249,132,201,206,217,68,57,151,45,105,165,154,60,111,134,31,62,25,253]]],\"c\":[[7,96,9,209,79,91,0,232,113,87,32,163,146,113,134,97,216,33,194,78,150,160,222,150,123,88,202,228,236,171,207,84],[24,35,80,218,45,162,14,142,79,158,53,50,121,190,131,213,93,167,137,57,199,76,147,158,230,19,101,211,85,123,193,94]]},\"post_state_digest\":[163,172,194,113,23,65,137,150,52,11,132,229,169,15,62,244,196,157,34,199,158,68,170,216,34,236,156,49,62,30,184,226],\"journal\":[81,0,0,0,73,32,107,110,111,119,32,121,111,117,114,32,112,114,105,118,97,116,101,32,105,110,112,117,116,32,105,115,32,103,114,101,97,116,101,114,32,116,104,97,110,32,51,32,97,110,100,32,108,101,115,115,32,116,104,97,110,32,51,52,44,32,97,110,100,32,73,32,99,97,110,32,112,114,111,118,101,32,105,116,33,0,0,0]}}"
w3bstream-coordinator  | time=2024-06-25T17:14:58.088Z level=INFO msg="task finished" project_id=10000 task_id=27
```
{% endtab %}

{% tab title="Halo2 Prover" %}
```
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "projectID": 10001,
    "projectVersion": "0.1",
    "data": "{\"private_a\":3, \"private_b\":4 }"
  }' \
  localhost:9000/message
  
```

The proof generation in this case should be pretty quick, the log in the first terminal should look like similar to the one below:

```
w3bstream-sequencer    | 200 |   15.621291ms |    192.168.65.1 | POST     "/message"
w3bstream-coordinator  | msg="dispatched a task" project_id=10001 task_id=29
w3bstream-prover       | msg="get a new task" project_id=10001 task_id=29
w3bstream-prover       | msg="acquire vm instance success" vm_type=halo2
w3bstream-coordinator  | msg=stdout proof=2c24ca70a6aef9655ed0626c4e6821abfee6a1b7569e2bc536f219d44b62cace22d31b906c7bf61a52dbb9a6cccb02b5e78d77ced622cf1477f276d25857ac09153bb58c1892b41aa7e99a5e2da023901a43e95785c52922db0983ea112dd30715a5f5ec4b609c30d7e410a9fd6fec23317f15a8ca30b2997c9cdddf207f0aeb162dd6c40b1dc440e9b34d9324f1c93c30cf72a47c043b46f27d26814908a58e0ef7b939e722a808bd6ecee2022eebd66fc2864b450dcf0b38e187d26c65229816db198b048b99a828d57b80306018816a0e257dd9d1d4c8ce1a25be66d9fa7d28d08acbdb02f7f929a9a7f10a7490b2701930d026429bd4387c03f707c3a712193c0381c41a744eec42ecdbb3d8a1ab04d2a5e03509cc59d274c0e7d1f7785f0568740ba807d7f6aef522fa01e393319ef3010bcb01d17b906ed0a1c2bb18072059efea92c5dc6523e5886ba334e8b734a085d4d800ea1c741bb62b661828480d2e90d65020c3a5e47a31d815e68a8ce5d2a0caf350f36b42587436fd16e5aa2070228b51ac58c197c16db4e9a6a8e2f41d76631c6e8268ccb1ac6d4c23c8b22eaf75f8a8a36a48561e342258f21a50fb1e2dc4519ba22de73d19a57f7129df0c5386fb9f6385d81e091ec0f65c6f5d74c02761e3d395bfab792afb5b5f30c22c4d10a854cd94c05f18fb56d34fb8c6abb68d7145165029d6a856b5518b58e51f7578055cfcd23315a3dff19938c75be23259347fd53447a0932981ee9732e912feed059f920689b9b289c77bf65b60a33b178f4787401b90da58d629f34c0017d2764fa0d8590508e753e097ba9157ea80ebeb7ee99e160617cb65ee4065721053522ab3899c8612db8317fd91b2a8828880359be34c2843f842bdf3b37ae81530eb544885a992a543e96e9408084c96f1c7d57da70543026e62fb96adf65905d6d0d2f21db313c6c37fc95dcdccb6b99e54b033729e7c34e028b7bb4cdff001a555a95bebdef533a9f53a3045a34d373da5dfdabc76f81d082f4c96964bdc12035ca90fe9dce6fcb77cae7d546c1532c5f9eb1bd45283a2e7acce174153d9234e6def14ee5ad2b2cda1889ec57bcba42985b4b1d023cbb891387e138d26ad06547e2a71824bb8f4f72bde760a6c41bf6d803a4a67c68de2930dc171557c422e7ae7d547101ef741421a9e14c05edd65173a9c616629f4bd534686dbb987c32a3b3e8b32ebfcf06aede25c541717452f7c1ff449b3b73477b113fbb915f293047c286c32a46b34cbd151cd8e4dfc2cc6e55dbfb7641af9686b8ea1d91b16c20303b0d2dc6725228bf239b5b263bae62030035481c297eccdee7b773be48b3e0c6b4f02ed95b2091f21bef9f2d44fff90d6ed08723a5bddd31d97c274a7db932f49470f32a44f13d362b7c74186b8d2d3261312f33e85e1e09fba3b56e32f4626481f135829a059e1124bc903620dbcdc85c8e8d5701c54fd1f12ac4fdb57361988622607509b6a5cfc3745231f7c67776c094283207618d35c63c53633a24926d9a9d13cd5e1e01c35d073af60470651701e6f0a5714ce8d2e8bba1415f85a1faf4818c226a82bb01efc9f8479bb5f333c02d20bec991217faf65e6844f6d7210379471ba1fd1b6772b0a6327b6611431a06fc34a4927c0c3610ebc278a575247055667c3bcb300c460975182525d4a12fa4c1c019bbc33b00b81bdca69c092af70597e5a0fb29e26d70030263c0d228fa4d5545f0e68671ba8828d3f45b622f8a81f908040a22a71e82adce9ca5b5a768fa911848fdc18c64fc8554cad3042ea15d6022343720f90c9d08b845e83f444f7fb36b72c6e212e4e5c75ac361260c817029d49988576e6905d65964cf9a5e93c43a8bcb0a6deca93fc8a2ee9f1b139c641abaa489251d4c265367285a5495e51e71d7a2250b0fc21d9d5e5a2cd81c24c38f0618b1a39cd9003c3a24b07ef7ea46e963055f05d7a6f8b0828981ae1b0390b33d71919e6d1281e8c10447ad007d4744ee1a958dd75e8e9b56ca795b
w3bstream-coordinator  | msg="task finished" project_id=10001 task_id=29
```
{% endtab %}

{% tab title="zkWASM Prover" %}
```
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "projectID": 10002,
    "projectVersion": "0.1",
    "data": "{\"private_input\":[1,1], \"public_input\":[2] }"
  }' \
  localhost:9000/message
```

After some time, the log in the first terminal should look like similar to the one below:

```
w3bstream-sequencer    | 200 |   17.208166ms |    192.168.65.1 | POST     "/message"
w3bstream-coordinator  | msg="dispatched a task" project_id=10001 task_id=30
w3bstream-prover       | msg="get a new task" project_id=10001 task_id=30
w3bstream-prover       | msg="acquire vm instance success" vm_type=halo2
w3bstream-coordinator  | msg=stdout proof=217ffa4c45a9db91cf1da3d3b18de088eba825d5c129e42834ab496d02f9b15c2d64a74cdb8232be47667c49a3bf653070aa00ee7f9a3d3f70b046923cc9bfe82234fc2563878005f92c50b1e03c89f1da25b206f7c852769de5ccd21247a3c8084ef36cb92fa85d193482f840e040dbf57b82a80eaf9f1b839425aa6d61b10d1934580dbfd66526767792dc3554e7741975e971a5967678e37cd025202b13272ed46935d817514a36d1689e86a8fbaa68c272864eb39420d65844473dd8bd781d03b4cb0e25efb02ad388bb6ecacd6fec84c2fccac3b59fee23dc6cc0d2ded915a679691f88286bdd34a689828329d7fc6373ccdeb07f69ce3fa9000471a669290a4ad928111be9c0d4d5da80010d7e96d290a45d19921cd5706a5b26f886af175d4b03468215bc970a65cfc9e74e17b2b3458024d0f6b5de60e259e2d83a3b2c69746789436fc886c246315a2219e6c5105703d5d666f23fd0ad5082e9535c1a44d40a4778aadaedb9524b96741e24666ef2f477c78b61c5230cf7754c6ad92ed06df716bee115f58a3382c9344cf5f774b781252afb28f4552737586fd52f042bb58fb24eed3523f84afdf036076b4673e4ffb8a8cd6d9c2c0c1e4ca2dd3d2edeb6fdc31b912df4bb57de64166a67d5d9a427f565fdedc19021463e7b361e0ffcc559ba0dab834b21011510f3d959fc36970b3984adaaae92f5d2ac08c7c91d4eb94eb47e9d931722caa00aaf4fe9ef7c0903ed279f8468cd3753914f92151c50a2038f20590eefff8493eb7262f3c9df41adf145974c55e361d2e0c1cfd00b88747d57287b8bc105cbb9218f01c8411c994617218416a60889e37b48aa521a9f46867de0e594f98be08653f764cdf8d6a1a3785e674041ac6d444b0d531b05fc36ff99c92812c9bd0bb66d7801335c11b63ba05b3dceb750b84de4eec86a2b2144b2ebd6210d4d426d45ab6a6edb49754e5017b3a1c5a2b10a7ace9e02e61059da9844e8b5d3dd69be1e86bfea3f3f1d16a3570e38094e073e89d5877bce2930aa4e4932c2ce3595264c32a96b92c10f1a784f5e7da7788e2cf2b796b25e02ad46939a2ac1b606945767247b28b412dab85c15e28388f6ee952001304072282e4ed4ca4b80235bb182ca93a085592b9465e59fbba7ab13fc7e295fa957301a76f4d4cd12b1c60078b2ba96002228956d33c59f0cb92e46605355078b19590faf22ec7e1f780d06646486dd15ee12140dec6260434f516ee8aed1164ff477210b6714b7ec0f4854c37601975ccf3de170ab59164e8588cb245a8a21da681f1af3944f681c655b0c972ce390468d235116337e92483029aac0c1b064250218010fb5206e8d34596608ae76e32ab5e45d523a252cd55402f6310bd16c10ad5f168448ee150c467ce18b1c8f60e49bc7570a85ac89018f00995d8bf20d1753760769f24bf159d143ae259c340059068547c12de5e0acc64629c071718ff9bda41c50913ea98616a30d70b11e3372aafefc7e93dd0740b9caf86675db5f23efed296585ba4da466e4fb2f4227a948c06fca8884f2fc4ec2087b9642a7fa1b121202c7dc2735f92e45e72d271bf9ec1a1d7c5985407feca7fe0472576e26b9d49d2181a817d1635bd25153f8a33c444e87aaa65dc8d7d3c033122c6ea90b63fde907548bbcf2242dcf85daae8b36aa51961a84f5c42c9212180ee1ca3fd9f72ad020b79a583ba1de88241d6ba8c04a64496d521b1f268260bbf44e08dc70d582d323a860e92c131694004ab61989e75abbf048a29f2f806f6dc86b6a730ae7f3290b9aa31f631291cdfba8b166e421fafb21c4b47270cdd841f49397fd5b1ccfc52aa03d79cc9bab16bad32ab648cae6eb891b2fefb35e77df551aa6e55fe65a912079efabd25c0bb68f8a28cf0d228f3e794e41c1ef143bbad05b0b384d6a58b00a06ae78640abbd850b9c267c8102b9f83ed64b6a6ac0050ee9927d8bd28e70113b960e2e78a10a6418743d9d2958f059ecfabc6ded3f636b1380cda368c6804
w3bstream-coordinator  | msg="task finished" project_id=10001 task_id=30
```
{% endtab %}
{% endtabs %}
