<template>
	<view class="content">
		<view v-for="item in recipeList" :key="item.day">
			<view>
				<text>{{item.day}}</text>
			</view>
			<view>
				<view>
					<text class="title">午餐：</text>
				</view>
				<text class="title">{{ item.lunch.map(v => v.name).join() }}</text>
			</view>
			<view>
				<view>
					<text class="title">晚餐:</text>
				</view>
				<text class="title">{{ item.dinner.map(v => v.name).join() }}</text>
			</view>
		</view>
	</view>
</template>

<script setup>
import { ref } from 'vue'
import { weightedShuffle } from '@/utils/utils'

// 荤菜列表
const meatList = new Array(100).fill('').map((_, i) => ({ name: `肉${i}`, weight: 1 }))
// 素材列表
const vegetableList = new Array(100).fill('').map((_, i) => ({ name: `蔬菜${i}`, weight: 1 }))

// 生成这一周的数据
const weekList = [
	{ day: '星期一', meatNum: 4, vegetableNum: 4 },
	{ day: '星期二', meatNum: 4, vegetableNum: 4 },
	{ day: '星期三', meatNum: 4, vegetableNum: 4 },
	{ day: '星期四', meatNum: 4, vegetableNum: 4 },
	{ day: '星期五', meatNum: 4, vegetableNum: 4 },
	{ day: '星期六', meatNum: 3, vegetableNum: 3 },
	{ day: '星期日', meatNum: 3, vegetableNum: 3 },
]

const recipeList = ref([])
// 给菜单随机排序
const shuffleMeatList = weightedShuffle(meatList)
const shuffleVegetableList = weightedShuffle(vegetableList)

// 根据每周每天的数量生成菜单
const createRecipe = (weekList, meatList, vegetableList) => {
	const recipeList = []
	weekList.forEach(v => {
		recipeList.push({
			day: v.day,
			lunch: [...meatList.splice(0, v.meatNum), ...vegetableList.splice(0, v.vegetableNum)],
			dinner: [...meatList.splice(0, v.meatNum), ...vegetableList.splice(0, v.vegetableNum)]
		})
	});

	return recipeList
}

recipeList.value = createRecipe(weekList, shuffleMeatList, shuffleVegetableList)

console.log(recipeList.value);

</script>
