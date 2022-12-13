<template>
  <div>
    <button
      v-if="!account.length"
      class="m-3 btn btn-primary"
      @click="connection"
    >
      Connect my wallet
    </button>
    <button v-else class="m-3 btn btn-error" @click="disconnection">
      Disconnect
    </button>
  </div>
</template>

<script>
import WalletConnectProvider from "@walletconnect/web3-provider";
import Web3 from "web3";

export default {
  data() {
    return {
      provider: null,
      account: "",
    };
  },
  created() {
    console.log("Welcome to your WalletConnect ~");
  },
  methods: {
    async connection() {
      //  Create Web3 instance
      console.log("Connection...");

      try {
        // instantiate WalletConnectProvider
        this.provider = new WalletConnectProvider({
          infuraId: process.env.INFURA_ID,
          qrcodeModalOptions: {
            desktopLinks: [
              "metmask",
              "ledger",
              "tokenary",
              "wallet",
              "wallet 3",
              "secuX",
              "ambire",
              "wallet3",
              "apolloX",
              "zerion",
              "sequence",
              "punkWallet",
              "kryptoGO",
              "nft",
              "riceWallet",
              "vision",
              "keyring",
            ],
            mobileLinks: [
              "rainbow",
              "metamask",
              "argent",
              "trust",
              "imtoken",
              "pillar",
            ],
          },
        });

        await this.provider.enable();
      } catch (error) {
        console.log(error);
      }

      console.log("Get account...");
      this.account = await this.getAccount();
      console.log("Connected to " + this.account);
    },

    async disconnection() {
      // Diconnec wallet and wipe local data
      await this.provider.disconnect();
      this.account = "";
      this.$emit("getAccountValue", "");

      console.log("Disconnected, good bye !");
    },

    async getAccount() {
      // Get account with web3
      const web3 = new Web3(this.provider);
      const accounts = await web3.eth.getAccounts();
      this.$emit("getAccountValue", accounts[0]);

      return accounts[0];
    },
  },
};
</script>
