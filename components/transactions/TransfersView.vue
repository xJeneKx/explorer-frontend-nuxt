<script setup>
import { getDateFromSeconds } from "~/helpers/date";

import Link from "~/components/elements/Link.vue";
import FormatAmount from "~/components/FormatAmount.vue";

defineProps(["listTransactions", "address"]);
</script>

<template>
  <div class="overflow-x-auto">
    <div class="inline-flex min-w-[1200px] bg-gray-100 font-bold py-1.5">
      <div class="w-60 text-center pr-2">Unit ID</div>
      <div class="w-44 text-center pr-2">Date</div>
      <div class="truncate w-64 text-center pr-2">From</div>
      <div class="w-14 pr-2" v-if="address">Direction</div>
      <div class="truncate w-64 text-center pr-2">To</div>
      <div class="w-52 pr-2 pl-3">Amount</div>
    </div>
    <TransitionGroup name="list" tag="div">
      <div class="min-w-max" v-for="t in listTransactions" :key="t.rowid">
        <div
          v-for="(transactionsGroupByAsset, unit) in t.transactionList"
          :key="unit"
          class="border-b flex flex-col"
        >
          <div
            v-for="(transactions, index) in transactionsGroupByAsset"
            :key="unit + '_' + index"
            class="inline-flex py-2"
          >
            <div class="truncate w-60 pr-2">
              <Link :type="'unit'" :link="unit">{{ unit }}</Link>
            </div>
            <div class="w-44 text-center pr-2">{{ getDateFromSeconds(t.timestamp) }}</div>
            <div>
              <div class="truncate w-64 mb-1 pr-2" v-for="from in transactions.from" :key="from">
                <span v-if="from === address">{{ from }}</span>
                <Link v-else :type="'address'" :link="from">{{ from }}</Link>
              </div>
            </div>
            <div class="w-14 pr-2" v-if="address">
              <span class="text-red-500" v-if="transactions.from.includes(address)">out</span>
              <span class="text-green-500" v-else>in</span>
            </div>
            <div>
              <div v-for="(amount, addr) in transactions.to" class="flex" :key="addr">
                <div class="truncate mb-1 w-64 pr-2 flex-none">
                  <span v-if="addr === address">{{ addr }}</span>
                  <Link v-else :type="'address'" :link="addr">{{ addr }}</Link>
                </div>
                <div class="w-full pl-2 mb-1 min-w-max">
                  <FormatAmount
                    :decimals="transactions.decimals"
                    :show-dollar="false"
                    :amount="amount"
                    :is-asset="true"
                  />
                  <span v-if="address">
                    <Link :type="'asset'" :link="transactions.assetName">{{
                      transactions.assetName
                    }}</Link>
                  </span>
                  <span v-else>
                    {{ transactions.assetName }}
                  </span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </TransitionGroup>
  </div>
</template>

<style scoped></style>
