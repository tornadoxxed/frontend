<script>
const API_ENDPOINT = import.meta.env.VITE_API_ENDPOINT;

export default {
  data() {
    return {
      searchString: "",
      doxxed: null,
    };
  },
  created() {
    this.getList();
  },
  methods: {
    async getList() {
      const executeResponse = await fetch(`${API_ENDPOINT}/lookup`, {
        method: "GET",
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
      });
      this.doxxed = await executeResponse.json();
    },
  },
  computed: {
    filteredDoxxed() {
      const filteredDoxxed =
        this.searchString === ""
          ? this.doxxed
          : this.doxxed.filter(
              (wo) =>
                Object.values(wo).join("").indexOf(this.searchString) !== -1
            );
      return filteredDoxxed;
    },
  },
};
</script>

<template>
  <h2>⚠️Your privacy might be at risk⚠️</h2>
  <p>Potential Link caused by matching total cross-pool amounts</p>
  <input v-model="searchString" placeholder="search" class="mb-1" />
  <table>
    <thead>
      <th>Depositor</th>
      <th>Recipient</th>
      <th>0.1 ETH</th>
      <th>1 ETH</th>
      <th>10 ETH</th>
      <th>100 ETH</th>
    </thead>
    <tbody>
      <tr v-for="address in filteredDoxxed">
        <td>
          <a
            :href="'https://blockscan.com/address/' + address.depositor_list"
            target="_blank"
            >{{ address.Recipient }}</a
          >
        </td>
        <td>
          <a
            :href="'https://blockscan.com/address/' + address.recipient_list"
            target="_blank"
            >{{ address.Depositor }}</a
          >
        </td>

        <td>{{ address["0.1 ETH"] }}</td>
        <td>{{ address["1 ETH"] }}</td>
        <td>{{ address["10 ETH"] }}</td>
        <td>{{ address["100 ETH"] }}</td>
      </tr>
    </tbody>
  </table>
</template>
