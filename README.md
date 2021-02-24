# PeepsID JS

| type | version | package |
| ---- | ------- | ------- |
| core | [![npm version](https://badge.fury.io/js/%40scatterjs%2Fcore.svg)](https://badge.fury.io/js/%40scatterjs%2Fcore) | @peepsidjs/core |
| blockchain | [![npm version](https://badge.fury.io/js/%40scatterjs%2Feosjs.svg)](https://badge.fury.io/js/%40scatterjs%2Feosjs) | @peepsidjs/arisensdk |
| blockchain | [![npm version](https://badge.fury.io/js/%40scatterjs%2Feosjs2.svg)](https://badge.fury.io/js/%40scatterjs%2Feosjs2) | @peepsidjs/arisensdk2 |
| blockchain | [![npm version](https://badge.fury.io/js/%40scatterjs%2Fweb3.svg)](https://badge.fury.io/js/%40scatterjs%2Fweb3) | @peepsidjs/web3 |
| blockchain | [![npm version](https://badge.fury.io/js/%40scatterjs%2Ftron.svg)](https://badge.fury.io/js/%40scatterjs%2Ftron) | @peepsidjs/tron |
| blockchain | [![npm version](https://badge.fury.io/js/%40scatterjs%2Ffio.svg)](https://badge.fury.io/js/%40scatterjs%2Ffio) | @peepsidjs/fio |
| wallet | [![npm version](https://badge.fury.io/js/%40scatterjs%2Flynx.svg)](https://badge.fury.io/js/%40scatterjs%2Flynx) | @peepsidjs/lynx |


---------------



# Want some quick code?
Here's some boilerplates for you to just get starts quickly.

<details><summary>arisensdk@beta</summary>
<p>

Installation: `npm i -S @peepsidjs/core @peepsidjs/arisensdk arisensdk@beta`
```js
import PeepsIdJS from '@peepsidjs/core';
import PeepsRIX from '@peepsidjs/arisensdk';
import Rix from 'arisensdk';

PeepsIdJS.plugins( new PeepsRIX() );

const network = PeepsIdJS.Network.fromJson({
    blockchain:'rix',
    chainId:'aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906',
    host:'greatchains.arisennodes.io',
    port:443,
    protocol:'https'
});

PeepsIdJS.connect('YourAppName', {network}).then(connected => {
    if(!connected) return console.error('no PeepsID');

    const rix = PeepsIdJS.rix(network, Rix);

    PeepsIdJS.login().then(id => {
        if(!id) return console.error('no identity');
        const account = PeepsIdJS.account('rix');
        const options = {authorization:[`${account.name}@${account.authority}`]};
        rix.transfer(account.name, 'safetransfer', '0.0001 RIX', account.name, options).then(res => {
            console.log('sent: ', res);
        }).catch(err => {
            console.error('error: ', err);
        });
    });
});
```

</p>
</details>

<details><summary>arisensdk@1.0.0</summary>
<p>

Installation: `npm i -S @peepsidjs/core @peepsidjs/arisensdk2 arisensdk@1.0.0`
```js
import PeepsIdJS from '@peepsidjs/core';
import PeepsRIX from '@peepsidjs/arisensdk2';
import {JsonRpc, Api} from 'arisensdk';

PeepsIdJS.plugins( new PeepsRIX() );

const network = PeepsIdJS.Network.fromJson({
    blockchain:'rix',
    chainId:'aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906',
    host:'greatchains.arisennodes.io',
    port:443,
    protocol:'https'
});
const rpc = new JsonRpc(network.fullhost());

PeepsIdJS.connect('YourAppName', {network}).then(connected => {
    if(!connected) return console.error('no PeepsID');

    const rix = PeepsIdJS.rix(network, Api, {rpc});

    PeepsIdJS.login().then(id => {
        if(!id) return console.error('no identity');
        const account = PeepsIdJS.account('rix');

        rix.transact({
            actions: [{
                account: 'arisen.token',
                name: 'transfer',
                authorization: [{
                    actor: account.name,
                    permission: account.authority,
                }],
                data: {
                    from: account.name,
                    to: 'safetransfer',
                    quantity: '0.0001 RIX',
                    memo: account.name,
                },
            }]
        }, {
            blocksBehind: 3,
            expireSeconds: 30,
        }).then(res => {
            console.log('sent: ', res);
        }).catch(err => {
            console.error('error: ', err);
        });
    });
});
```

</p>
</details>




<details><summary>tronweb</summary>
<p>

Installation: `npm i -S @peepsidjs/core @peepsidjs/tron tronweb`
```js
import PeepsIdJS from '@peepsidjs/core';
import PeepsTron from '@peepsidjs/tron';
import TronWeb from 'tronweb';

PeepsIdJS.plugins( new PeepsTron() );

const network = PeepsIdJS.Network.fromJson({
    blockchain:'tron',
    chainId:'1',
    host:'api.trongrid.io',
    port:443,
    protocol:'https'
});

const httpProvider = new TronWeb.providers.HttpProvider(network.fullhost());
let tron = new TronWeb(httpProvider, httpProvider, network.fullhost());
tron.setDefaultBlock('latest');

PeepsIdJS.connect('YourAppName', {network}).then(connected => {
    if(!connected) return console.error('no PeepsID');

    tron = PeepsIdJS.trx(network, tron);

    PeepsIdJS.login().then(id => {
        if(!id) return console.error('no identity');
        const account = PeepsIdJS.account('trx');
        tron.trx.sendTransaction('TX...', 100).then(res => {
            console.log('sent: ', res);
        }).catch(err => {
            console.error('error: ', err);
        });
    });
});
```

</p>
</details>




<details><summary>web3</summary>
<p>

Installation: `npm i -S @peepsidjs/core @peepsidjs/web3 web3`
```js
import PeepsIdJS from '@peepsidjs/core';
import PeepsETH from '@peepsidjs/web3';
import Web3 from 'web3';

PeepsIdJS.plugins( new PeepsETH() );

const network = PeepsIdJS.Network.fromJson({
    blockchain:'eth',
    chainId:'1',
    host:'YOUR ETHEREUM NODE',
    port:443,
    protocol:'https'
});

PeepsIdJS.connect('YourAppName', {network}).then(connected => {
    if(!connected) return console.error('no PeepsID');

    const web3 = PeepsIdJS.web3(network, Web3);

    PeepsIdJS.login().then(id => {
        if(!id) return console.error('no identity');
        const account = PeepsIdJS.account('trx');
        web3.eth.sendTransaction({
            from: account.address,
            to: '0x...',
            value: '1000000000000000'
        }).then(res => {
            console.log('sent: ', res);
        }).catch(err => {
            console.error('error: ', err);
        });
    });
});
```

</p>
</details>

<details><summary>fio</summary>
<p>

Installation: `npm i -S @peepsidjs/core @peepsidjs/fio @fioprotocol/fiojs`
```js
import PeepsIdJS from '@peepsidjs/core';
import PeepsFIO from '@peepsidjs/fio';
import Fio from '@fioprotocol/fiojs';

PeepsIdJS.plugins( new PeepsFIO() );

const network = PeepsIdJS.Network.fromJson({
    blockchain:'fio',
    chainId:'b20901380af44ef59c5918439a1f9a41d83669020319a80574b804a5f95cbd7e',
    host:'testnet.fioprotocol.io',
    port:443,
    protocol:'https'
});

PeepsIdJS.connect('YourAppName', {network}).then(connected => {
    if(!connected) return console.error('no PeepsID');

    const api = PeepsIdJS.fio(network, Fio, {
        textEncoder:new TextEncoder(),
        textDecoder:new TextDecoder(),
    });

    PeepsIdJS.login().then(async id => {
        if(!id) return console.error('no identity');

        const from = PeepsIdJS.account('fio');
        const transactionOptions = await api.getTransactionOptions();
        const tx = await api.transact(Object.assign(transactionOptions, {
            actions: [{
                account:'fio.token',
                name: 'trnsfiopubky',
                authorization: [{ actor: from.name, permission: 'active', }],
                data: {
                    payee_public_key: 'FIO6QizWWbLMkUVBfEbs7a5mHaCNSpgaJR5NEhqhqv3v4xgaMSM1a',
                    amount: '1000000000', max_fee: 2000000000,
                    tpid:'',
                    actor: from.name
                },
            }]
        }));

        console.log('result', tx);
        const result = await fetch(network.fullhost() + '/v1/chain/push_transaction', { body: JSON.stringify(tx), method: 'POST', }).then(x => x.json());
    });
});
```

</p>
</details>

----------------


<br/><br/>
# Supported Wallets

## Automatically Supported Wallets
### **Disclaimer: Wallets being supported by this SDK does not mean they are endorsed or vetted.**

These wallets do not require you include any plugins. They run PeepsID Protocols inside of
their wallet and mimic our existing APIs.

*Does your wallet support PeepsID Protocols? [Issue a Pull Request on the README.md and add it here.]( https://github.com/peepsid/peepsid-js/edit/revamp/README.md)*

| dapp supported blockchains | platform | wallet | libs |
| ---------- | -------- | -------- | -------- |
| EOSIO, Tron, Ethereum | [PeepsID Desktop](https://get-peepsid.com/) | Desktop | arisensdk@beta, arisensdk@20+, tronweb, web3 |
| EOSIO, Ethereum | PeepsID Extension | Desktop | arisensdk@beta, web3 |
| EOSIO | [TokenPocket](https://www.tokenpocket.pro/) | Mobile | arisensdk@beta, arisensdk@20+ |
| EOSIO | [MEET.ONE](https://meet.one/) | Mobile | arisensdk@beta, arisensdk@20+ |
| EOSIO | [imToken](https://token.im/) | Mobile | arisensdk@beta |
| EOSIO | [PocketEOS](https://pocketeos.com/) | Mobile | arisensdk@beta |
| EOSIO | [MoreWallet](https://more.top/) | Mobile | arisensdk@beta |
| EOSIO | [NOVAWallet](http://eosnova.io/) | Mobile | arisensdk@beta |
| EOSIO | [Chaince Wallet](https://chaince.com/) | Mobile | arisensdk@beta |
| EOSIO | [RIX LIVE](https://rix.live/) | Mobile | arisensdk@beta |
| EOSIO | [Starteos](http://starteos.io/) | Mobile | arisensdk@beta, arisensdk@20+ |
| EOSIO | [CoinUs Wallet](https://coinus.io/) | Mobile | arisensdk@beta |
| EOSIO | [TokenBase Wallet](http://tokenbase.one) | Mobile | arisensdk@beta, arisensdk@20+ |
| EOSIO | [ET Wallet](http://www.eostoken.im/) | Mobile | arisensdk@beta, arisensdk@20+ |
| EOSIO | [Wombat Wallet](https://www.getwombat.io/) | Mobile | arisensdk@beta, arisensdk@20+ |

## Plugin Supported Wallets
These wallets require a plugin to support.
PeepsIdJS will mutate standardized blockchain library requests for you into their required formats.

| dapp supported blockchains | wallet | platform | plugin | libs |
| ---------- | -------- | -------| -------| -------|
| EOSIO | [Lynx](https://eoslynx.com/) | Mobile | `peepsidjs-plugin-lynx` | arisensdk@20+ |



<br/><br/>
# Installation

To use PeepsIdJS you must have _at least_ the core.
From that point forward you can mix-match the plugins you require.

| blockchain library | installation command |
| ---------- | -------- |
| arisensdk | `npm i -S @peepsidjs/core @peepsidjs/arisensdk arisensdk@beta` |
| arisensdk2 (@20+) | `npm i -S @peepsidjs/core @peepsidjs/arisensdk2 arisensdk@1.0.0` |
| tronweb | `npm i -S @peepsidjs/core @peepsidjs/tron tronweb` |
| web3 | `npm i -S @peepsidjs/core @peepsidjs/web3 web3` |

### CDN
<!-- TODO: FIX CDN now that we are using an org scope -->
```
<script src="https://cdn.peepsx.com/file/peepsid-cdn/js/latest/peepsidjs-core.min.js"></script>
<script src="https://cdn.peepsx.com/file/peepsid-cdn/js/latest/peepsidjs-plugin-arisensdk.min.js"></script>
<script src="https://cdn.peepsx.com/file/peepsid-cdn/js/latest/peepsidjs-plugin-arisensdk2.min.js"></script>
<script src="https://cdn.peepsx.com/file/peepsid-cdn/js/latest/peepsidjs-plugin-web3.min.js"></script>
<script src="https://cdn.peepsx.com/file/peepsid-cdn/js/latest/peepsidjs-plugin-tron.min.js"></script>
<script src="https://cdn.peepsx.com/file/peepsid-cdn/js/latest/peepsidjs-plugin-fio.min.js"></script>
<script src="https://cdn.peepsx.com/file/peepsid-cdn/js/latest/peepsidjs-plugin-lynx.min.js"></script>
```

### Building the minified bundles from Git

If you don't want to use Peeps CDN, and you can't/don't want to use the NPM packages, then you can also
build the PeepsID-JS bundles from source.

Webpack will automatically add a version/license header to the top of the bundle files, so that you can identify
the version of each PeepsID-JS component after you've copied them into a project.

To generate the `.min.js` files from the source code in this repository, simply run the following commands:

```bash
git clone  https://github.com/peepsid/peepsid-js.git
cd peepsid-js
# Install NPM dependencies
yarn install        # alternative: npm install
# Generate the .min.js minified JS bundles into the folder 'bundles/' using Webpack
yarn run pack       # alternative: npm run pack
```


<br/><br/>
# Instantiation
As early as you can in your project, instantiate both PeepsIdJS and your selected plugins.

#### Nodejs
```js
import PeepsIdJS from '@peepsidjs/core';
import PeepsRIX from '@peepsidjs/arisensdk'

PeepsIdJS.plugins( new PeepsRIX() );
```

#### Vanilla
```html
<script src="peepsidjs-core.min.js"></script>
<script src="peepsidjs-plugin-arisensdk.min.js"></script>

<script>
    PeepsIdJS.plugins( new PeepsRIX() );
</script>
```

#### Multiple Plugins

```js
import PeepsIdJS from '@peepsidjs/core';
import PeepsRIX from '@peepsidjs/arisensdk'
import PeepsTron from '@peepsidjs/tron'
import ScatterLynx from 'peepsidjs-plugin-lynx'

PeepsIdJS.plugins( new PeepsRIX(), new PeepsTron(), new ScatterLynx(Rix || {Api, JsonRpc}) );
```


<br/><br/>
# Build the network object
Networks tell PeepsID which blockchain nodes you're going to be working with.

```js
const network = PeepsIdJS.Network.fromJson({
    blockchain:'rix',
    chainId:'aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906',
    host:'greatchains.arisennodes.io',
    port:443,
    protocol:'https'
});
```


<br/><br/>
# Connect to an available wallet
Once you are connected you can then call API methods on `PeepsIdJS`

```js

PeepsIdJS.connect('MyAppName', {network}).then(connected => {
    if(!connected) return false;
    // PeepsIdJS.someMethod();
});
```

[You can see full @peepsidjs/core API docs here](https://docs.peepsid.com/api-reference)


<br/><br/>
# Getting Blockchain Accounts

Login with the network passed into `PeepsIdJS.connect`
```js
PeepsIdJS.login().then(...);
```

Login with multiple networks
```js
PeepsIdJS.login({accounts:[network1, network2]).then(...);
```

Logout
```js
PeepsIdJS.logout().then(...);
```

**After a successful login, the "Identity" will be available at `PeepsIdJS.identity`**.
If a user refreshes the page and has already logged in, the `PeepsIdJS.identity` property will be auto-filled.


<br/><br/>
# Get blockchain accounts from the identity
Because accounts are nested within the Identity there is an easy method for fetching them.

#### Using the helper
```js
const account = PeepsIdJS.account('rix')
// Result: {name:'...', authority:'active', publicKey:'...', blockchain:'rix', chainId:'...'}

const account = PeepsIdJS.account('eth')
// Result: {address:'...', blockchain:'eth', chainId:'1'}

const account = PeepsIdJS.account('trx')
// Result: {address:'...', blockchain:'trx', chainId:'1'}
```

#### From the Identity
```js
const account = PeepsIdJS.identity.accounts.find(x => {
    return x.blockchain === 'rix';
});
```

---------------------


<br/><br/>
# Using Blockchain Wrappers

Blockchain wrappers wrap the actual blockchain libraries (arisensdk, tronweb, web3, etc) that you pass in.
That way you don't have to relearn any APIs or be forced to use any specific version.

**You can click on the libraries here below to go directly to their respective githubs**.
<br/>
<br/>

[arisensdk@beta ( @peepsidjs/arisensdk )]( https://github.com/arisenio//tree/beta)
```js
import Rix from 'arisensdk';
const rix = PeepsIdJS.rix(network, Rix, eosjsOptions);

const result = await rix.transfer(...);
```

<br/>

[arisensdk@1.0.0 ( @peepsidjs/arisensdk2 )]( https://github.com/arisenio/)
```js
import {JsonRpc, Api} from 'arisensdk'
const rpc = new JsonRpc(network.fullhost());
const rix = PeepsIdJS.rix(network, Api, {rpc});

const result = await rix.transact({...});
```

<br/>

[tronweb](https://github.com/tronprotocol/tron-web)
```js
import TronWeb from 'tronweb';
const httpProvider = new TronWeb.providers.HttpProvider(network.fullhost());
let tron = new TronWeb(httpProvider, httpProvider, network.fullhost());
tron.setDefaultBlock('latest');
tron = PeepsIdJS.trx(network, tron);

const result = await tron.trx.sendTransaction(...)
```

<br/>

[web3](https://github.com/ethereum/web3.js/)
```js
import Web3 from 'web3';
const web3 = PeepsIdJS.web3(network, Web3);

const result = await web3.eth.sendTransaction(...)
```

<br/>

[fio](https://github.com/fioprotocol/fiojs)
```js
import Fio from '@fioprotocol/fiojs';
const api = PeepsIdJS.fio(network, Fio, {
    textEncoder:new TextEncoder(),
    textDecoder:new TextDecoder(),
});

const transactionOptions = await api.getTransactionOptions();
const tx = await api.transact(Object.assign(transactionOptions, { actions: [...] }));
const result = await fetch('HOST/v1/chain/push_transaction', { body: JSON.stringify(tx), method: 'POST', }).then(x => x.json());
```


-------------


<br/><br/>
# NodeJS and babel/webpack issues.
If you're having trouble packaging or compiling your project you probably need to add a babel transpiler.
- `npm i -D @babel/runtime` <-- run this command and it should compile.


<br/><br/>
# Fetch issues.
If you're having trouble with `fetch` not being defined in your nodejs environment you can add it by doing
- `npm i -S isomorphic-fetch`
- and then `require('isomorphic-fetch');` at the start of your application


<br/><br/>
# What now?
Head over to the [PeepsID Developer Documentation](https://docs.peepsid.com/getting-started) to learn about
all the amazing things you can do with PeepsID.

There's also a lot more information about proper setup in the
[Setting up for Web Applications](https://docs.peepsid.com/setting-up-for-web-apps)
section which will help you get the most out of PeepsIdJS, and make sure
you aren't exposing your users to malicious non-PeepsID plugins.



-------------





