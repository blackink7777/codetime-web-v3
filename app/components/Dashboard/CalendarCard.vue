<script setup lang="ts">
const props = defineProps<{
  data: {
    duration: number
    time: string
    by?: string
  }[]
  loading?: boolean
}>()
const data = computed(() => props.data)

const pAllData = useProcessedData(data)
const totalMinutes = useTotalMinutes(data)
const todayMinutes = useTodayMinutes(data)
const currentStreak = useCurrentStreak(pAllData)
const maxStreak = useMaxStreak(pAllData)
const t = useI18N()
</script>

<template>
  <CardBase
    class="min-h-250px"
    :loading="loading"
  >
    <div class="p-2 flex flex-col">
      <div
        class="flex flex-wrap gap-2 sm:flex-nowrap children:flex-grow children:sm:flex-basis-[200px] min-w-0"
      >
        <DashboardDataBody
          :title="t.dashboard.overview.statistic.timeTotal"
          :value="getDurationString(totalMinutes * 60 * 1000)"
        />
        <DashboardDataBody
          :title="t.dashboard.overview.statistic.timeToday"
          :value="getDurationString(todayMinutes * 60 * 1000)"
        />
        <DashboardDataBody
          :title="t.dashboard.overview.statistic.currentStreak"
          :value="formateDays(currentStreak)"
        />
        <DashboardDataBody
          :title="t.dashboard.overview.statistic.longestStreak"
          :value="formateDays(maxStreak)"
        />
      </div>
      <YearCalendarChart
        :data="pAllData"
      />
    </div>
  </CardBase>
</template>
