<template>
  <div class="container">
    <div class="row">
      <div class="col-lg-12">
        <div class="py-5 text-center">
          <img class="d-block mx-auto mb-4" src="https://st4.depositphotos.com/4060975/20056/v/1600/depositphotos_200568474-stock-illustration-symmetric-encryption-line-vector-icon.jpg" alt="" width="72" height="72">
          <h2>Criptografia Simétrica</h2>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-lg-12">
        <form>
          <div class="form-group">
            <label for="exampleInputPassword1">Chave Simétrica:</label>
            <input type="text" class="form-control form-control-lg" v-model="cipher2.secret">
          </div>
          <div class="form-group">
            <label for="exampleInputPassword1">Texto:</label>
            <input type="text" class="form-control form-control-lg" v-model="cipher2.text">
          </div>
          <div class="form-group">
            <label for="exampleInputPassword1">Texto Criptografado:</label>
            <input type="text" class="form-control form-control-lg" v-model="cipher2.cipherText">
          </div>
          <div class="form-group">
            <div class="btn-group" role="group">
              <button type="button" class="btn btn-lg btn-primary" @click="encrypt2()">Criptografar</button>
              <button type="button" class="btn btn-lg btn-secondary" @click="decrypt2()">Descriptografar</button>
            </div>
          </div>
        </form>
      </div>
    </div>

    <div class="row">
      <div class="col-lg-12">
        <div class="py-5 text-center">
          <img class="d-block mx-auto mb-4" src="https://www.clipartwiki.com/clipimg/detail/166-1667107_public-domain-key-icon.png" alt="" width="72" height="72">
          <h2>Criptografia Assimétrica</h2>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-lg-12">
        <form>
          <div class="form-group">
            <label for="exampleInputEmail1">Private Key:</label>
            <input type="email" class="form-control form-control-lg" v-model="keyInfo2.privateKey">
          </div>
          <div class="form-group">
            <label for="exampleInputPassword1">Public Key:</label>
            <input type="text" class="form-control form-control-lg" v-model="keyInfo2.publicKey">
          </div>
          <div class="form-group">
            <div class="btn-group" role="group">
              <button type="button" class="btn btn-lg btn-primary" @click="generateNewKeys2()">Gerar</button>
            </div>
          </div>
          <div class="form-group">
            <label for="exampleInputPassword1">Texto:</label>
            <input type="text" class="form-control form-control-lg" v-model="cipher3.text">
          </div>
          <div class="form-group">
            <label for="exampleInputPassword1">Texto Criptografado:</label>
            <input type="text" class="form-control form-control-lg" v-model="cipher3.cipherText">
          </div>
          <div class="form-group">
            <div class="btn-group" role="group">
              <button type="button" class="btn btn-lg btn-primary" @click="encrypt3()">Criptografar (Chave Pública)</button>
              <button type="button" class="btn btn-lg btn-secondary" @click="decrypt3()">Descriptografar (Chave Privada)</button>
            </div>
          </div>
        </form>
      </div>
    </div>

    <div class="row">
      <div class="col-lg-12">
        <div class="py-5 text-center">
          <img class="d-block mx-auto mb-4" src="https://image.flaticon.com/icons/png/512/3/3856.png" alt="" width="72" height="72">
          <h2>Criptografia Simétrica Segura</h2>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-lg-12">
        <form>
          <div class="form-group">
            <label for="exampleInputEmail1">Private Key:</label>
            <input type="email" class="form-control form-control-lg" v-model="keyInfo.privateKey" @change="clearInfo()">
          </div>
          <div class="form-group">
            <label for="exampleInputPassword1">Public Key:</label>
            <input type="text" class="form-control form-control-lg" v-model="keyInfo.publicKey">
          </div>
          <div class="form-group">
            <label for="exampleInputPassword1">Eth Address:</label>
            <input type="text" class="form-control form-control-lg" v-model="keyInfo.ethAddress">
          </div>
          <div class="form-group">
            <div class="btn-group" role="group">
              <button type="button" class="btn btn-lg btn-primary" @click="generateNewKeys()">Gerar</button>
              <button type="button" class="btn btn-lg btn-secondary" @click="importFromPrivateKey()">Importar</button>
            </div>
          </div>

        </form>
      </div>
    </div>
    <div class="row">
      <div class="col-lg-12">
        <form>
          <div class="form-group">
            <label for="exampleInputEmail1">Public Key (Destino):</label>
            <input type="email" class="form-control form-control-lg" v-model="cipher.publicKey">
          </div>
          <div class="form-group">
            <label for="exampleInputPassword1">Chave Simétrica:</label>
            <input type="text" class="form-control form-control-lg" v-model="cipher.secret">
            <button type="button" class="btn btn-lg btn-primary" @click="computeSecret()">Gerar Chave</button>
          </div>
          <div class="form-group">
            <label for="exampleInputPassword1">Texto:</label>
            <input type="text" class="form-control form-control-lg" v-model="cipher.text">
          </div>
          <div class="form-group">
            <label for="exampleInputPassword1">Texto Criptografado:</label>
            <input type="text" class="form-control form-control-lg" v-model="cipher.cipherText">
          </div>
          <div class="form-group">
            <div class="btn-group" role="group">
              <button type="button" class="btn btn-lg btn-primary" @click="encrypt()">Criptografar</button>
              <button type="button" class="btn btn-lg btn-secondary" @click="decrypt()">Descriptografar</button>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
const crypto = require('crypto')
const keccak256 = require('keccak256')
const eccrypto = require('eccrypto')
const Cryptr = require('cryptr')
const EthCrypto = require('eth-crypto')

export default {
  data () {
      return {
        keyInfo: {
          privateKey: '',
          publicKey: '',
          ethAddress: ''
        },
        keyInfo2: {
          privateKey: '',
          publicKey: '',
          ethAddress: ''
        },
        cipher: {
          publicKey: '',
          secret: '',
          text: '',
          cipherText: ''
        },
        cipher2: {
          secret: '',
          text: '',
          cipherText: ''
        },
        cipher3: {
          secret: '',
          text: '',
          cipherText: ''
        }
      }
  },

  methods: {
    generate () {
      const o = crypto.createECDH('secp256k1');
      o.generateKeys()
      return this.getInfo(o);
    },

    getFromPrivateKey (privateKey) {
      const o = crypto.createECDH('secp256k1');
      o.setPrivateKey(Buffer.from(privateKey, 'hex'));
      return this.getInfo(o);
    },

    getInfo (obj) {
      const publicKey = obj.getPublicKey();
      const ethPublicKey = keccak256(publicKey.slice(1)).slice(-20).toString('hex')

      return {
        privateKey: obj.getPrivateKey('hex'),
        publicKey: publicKey.toString('hex'),
        ethAddress: '0x' + ethPublicKey.toString('hex')
      }
    },

    generateNewKeys () {
      this.keyInfo = this.generate()
    },

    generateNewKeys2 () {
      this.keyInfo2 = this.generate()
    },

    importFromPrivateKey () {
      try {
        this.keyInfo = this.getFromPrivateKey(this.keyInfo.privateKey)
      } catch {
        alert('Private key é inválida');
      }
    },

    computeSecret () {
      const o = crypto.createECDH('secp256k1');
      o.setPrivateKey(Buffer.from(this.keyInfo.privateKey, 'hex'));
      this.cipher.secret = o.computeSecret(Buffer.from(this.cipher.publicKey, 'hex')).toString('hex')
    },

    clearInfo () {
      this.keyInfo.publicKey = '';
      this.keyInfo.ethAddress = '';
    },

    encrypt () {
      var cryptr = new Cryptr(this.cipher.secret);
      this.cipher.cipherText = cryptr.encrypt(this.cipher.text);
    },

    decrypt () {
      var cryptr = new Cryptr(this.cipher.secret);
      this.cipher.text = cryptr.decrypt(this.cipher.cipherText);
    },

    encrypt2 () {
      var cryptr = new Cryptr(this.cipher2.secret);
      this.cipher2.cipherText = cryptr.encrypt(this.cipher2.text);
    },

    decrypt2 () {
      var cryptr = new Cryptr(this.cipher2.secret);
      this.cipher2.text = cryptr.decrypt(this.cipher2.cipherText);
    },

    async encrypt3 () {
      const encrypted = await EthCrypto.encryptWithPublicKey(
          this.keyInfo2.publicKey,
          this.cipher3.text
      );

      this.cipher3.cipherText = EthCrypto.cipher.stringify(encrypted);
    },

    async decrypt3 () {
      this.cipher3.text = await EthCrypto.decryptWithPrivateKey(
              this.keyInfo2.privateKey,
              EthCrypto.cipher.parse(this.cipher3.cipherText)
          );
    },
  }
}
</script>

<style>

</style>
