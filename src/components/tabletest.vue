<template>
	<div class="box">
		<el-table :data="filterTableData" style="width: 100%" max-height="250">
			<el-table-column fixed prop="date" label="Date" width="120" />
			<el-table-column prop="name" label="Name" width="80" />
			<el-table-column prop="state" label="State" width="100" />
			<el-table-column fixed="right" label="Operations" width="180">
				<template #header>
					<el-input
						v-model="search"
						size="small"
						placeholder="Type to search"
					/>
				</template>
				<template #default="scope">
					<el-button size="small" @click="handleEdit(scope.$index, scope.row)"
						>Edit</el-button
					><el-button
						size="small"
						type="danger"
						@click="deleteRow(scope.$index)"
						>Delete</el-button
					>
				</template>
			</el-table-column>
		</el-table>
		<el-button class="mt-4" style="width: 100%" @click="onAddItem"
			>Add Item</el-button
		>
	</div>
</template>

<script>
import { ref, computed } from 'vue'
import dayjs from 'dayjs'
export default {
	name: 'tabletest',
	setup() {
		const now = new Date()
		let search = ref('')
		const tableData = ref([
			{
				date: '2016-05-01',
				name: 'A',
				state: 'Ahome',
			},
			{
				date: '2016-05-02',
				name: 'B',
				state: 'Bhome',
			},
			{
				date: '2016-05-03',
				name: 'C',
				state: 'Chome',
			},
		])
		const handleEdit = (index, row) => {
			let aname = prompt('请输入修改后的name')
			let state = prompt('请输入修改后的state')
			row.name = aname || row.name
			row.state = state || row.state
		}
		const deleteRow = index => {
			console.log(index)
			tableData.value.splice(index, 1)
		}
		const onAddItem = () => {
			now.setDate(now.getDate() + 1)
			let aname = prompt('请输入name')
			let astate = prompt('请输入state')
			tableData.value.push({
				date: dayjs(now).format('YYYY-MM-DD'),
				name: aname,
				state: astate,
			})
		}
		const filterTableData = computed(() =>
			tableData.value.filter(
				data =>
					!search.value ||
					data.name.toLowerCase().includes(search.value.toLowerCase())
			)
		)
		return {
			filterTableData,
			search,
			deleteRow,
			onAddItem,
			tableData,
			handleEdit,
		}
	},
}
</script>

<style>
.hr {
	margin-top: 2px;
	border-bottom: 3px solid black;
	width: 100%;
}
.button {
	width: 40%;
}
.box {
	margin: 100px auto;
	width: 80%;
	border: 2px solid black;
}
</style>
