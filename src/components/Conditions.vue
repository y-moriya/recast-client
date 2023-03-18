<script lang="ts">
import { defineComponent } from "vue";
import { useQuery } from "@tanstack/vue-query";

import type { Condition } from "../types";

const fetcher = async (): Promise<Condition[]> =>
  await fetch("http://localhost:8082/conditions?order=id.desc&limit=1").then((response) =>
    response.json()
  );

export default defineComponent({
  name: "ConditionsList",
  setup() {
    const { isLoading, isError, isFetching, data, error, refetch } = useQuery({
      queryKey: ["conditions"],
      queryFn: fetcher,
      refetchInterval: 1000
    });

    return { isLoading, isError, isFetching, data, error, refetch };
  },
});
</script>

<template>
  <h1>Conditions</h1>
  <div v-if="isLoading">Loading...</div>
  <div v-else-if="isError">An error has occurred: {{ error }}</div>
  <div v-else-if="data">
    <ul>
      <li v-for="item in data" :key="item.id">
        {{ item.domain }} / {{ item.board }} / {{ item.keyword }}
      </li>
    </ul>
  </div>
</template>
