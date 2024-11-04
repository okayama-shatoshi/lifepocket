<template>
	<div class="tx-card-extension">
                <!-- f-begin -->
                <div v-if="fileName || folderName" class="flex-row" style="justify-content:space-between;">
                    <div v-if="fileName">File: {{ fileName }}</div>
                    <div v-if="folderName">Folder: {{ folderName }}</div>
                </div>
                <!-- f-end -->
		<div v-if="tx.tags.length || tx.data_size" class="flex-row" style="justify-content:space-between;">
			<div v-if="tx.tags.length">Tags:</div>
			<div v-if="tx.data_size">Data: {{ humanFileSize(tx.data_size) }}</div>
		</div>
		<ul v-if="tx.tags.length" class="tags secondary-text no-scrollbar">
			<li v-for="tag in tx.tags">
				{{ tag.name + ' | ' + tag.value }}
			</li>
		</ul>
	</div>
</template>



<script setup>
import { computed } from 'vue'
import { unpackTags } from '@/functions/Transactions'
import { humanFileSize } from '@/functions/Utils'
import ArweaveStore, { arweave } from '@/store/ArweaveStore'
import InterfaceStore from '@/store/InterfaceStore'

const props = defineProps(['tx'])

// owner - verify
// tags
// data_size
// data
// reward

const tags = computed(() => unpackTags(props.tx.tags, { lowercase: true }))
const fileName = computed(() => tags.value['file-name'] || tags.value['File-Name'])
const folderName = computed(() => tags.value['folder-name'] || tags.value['Folder-Name'])

const verticalElement = computed(() => InterfaceStore.breakpoints.verticalLayout)
</script>


<style scoped>
.tags {
	white-space: normal;
	overflow-x: auto;
}

ul {
	padding-inline-start: 1em;
	margin-block-start: 0;
}

li {
	white-space: nowrap;
}
</style>
