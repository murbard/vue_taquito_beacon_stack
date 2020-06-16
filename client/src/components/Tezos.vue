<template>
  <div>
    <h1>Boo {{ balance }}</h1>
  </div>
</template>

<script>
import { Tezos } from "@taquito/taquito";
import { BeaconWallet } from "@taquito/beacon-wallet";

Tezos.setProvider({ rpc: "https://api.tez.ie/rpc/mainnet" });

export default {
  name: "Tezos",
  data() {
    return {
      balance: -1
    };
  },
  created() {
    Tezos.tz.getBalance("tz1irJKkXS2DBWkU1NnmFQx1c1L7pbGg4yhk").then(x => {
      console.log(x);
      this.balance = x;
      var beaconWallet = new BeaconWallet({ name: "MyVueApp" });
      /* beaconWallet.init().then(
          beaconwallet => { console.log(beaconwallet); Tezos.setProvider({ wallet: beaconwallet }) }
        ) */
      Tezos.setWalletProvider(beaconWallet);
      beaconWallet
        .requestPermissions()
        .then(permissions =>
          console.log("user's address:", permissions.address)
        );
    });
  }
};
</script>
