<template>
  <div>
    <b-table
      :items="history"
      :fields="list_fields"
      :sort-desc="true"
      sort-by="blocks"
      striped
      hover
      stacked="sm"
    >
      <!-- Column: Height -->
      <template #cell(height)="data">
        <router-link :to="`./blocks/${data.item.block.header.height}`">
          {{ data.item.block.header.height }}
        </router-link>
      </template>
      <template #cell(hash)="data">
        <small>{{ data.item.block_id.hash }}</small>
      </template>
      <template #cell(time)="data">
        {{ formatTime(data.item.block.header.time) }}
      </template>
      <template #cell(proposer)="data">
        {{ formatProposer(data.item.block.header.proposer_address) }}
      </template>
      <template #cell(txs)="data">
        {{ length(data.item.block.data.txs) }}
      </template>
    </b-table>
  </div>
</template>

<script>
import {
  VBTooltip, BTable, BAvatar, BButton,
} from 'bootstrap-vue'
import { getLocalTxHistory } from '@/libs/utils'

export default {
  components: {
    BTable, BAvatar, BButton,
  },
  directives: {
    'b-tooltip': VBTooltip,
  },
  data() {
    return {
      fields: [
        { key: 'chain', label: 'BLOCKCHAIN' },
        { key: 'op', label: 'ACTION' },
        { key: 'hash', label: 'TX HASH' },
        { key: 'time', label: 'TIME' },
      ],
      history: [],
    }
  },
  created() {
    this.history = getLocalTxHistory()
  },
  methods: {
    clear() {
      this.history = []
      localStorage.setItem('txHistory', [])
    },
  },
}
</script>
