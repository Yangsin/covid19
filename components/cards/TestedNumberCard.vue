<template>
  <v-col cols="12" md="6" class="DataCard">
    <time-stacked-bar-chart
      :title="$t('検査実施件数')"
      :title-id="'number-of-tested'"
      :chart-id="'time-stacked-bar-chart-inspections'"
      :chart-data="inspectionsGraph"
      :date="inspectionsSummary.last_update"
      :items="inspectionsItems"
      :labels="inspectionsLabels"
      :unit="$t('件.tested')"
      :data-labels="inspectionsDataLabels"
      :url="'http://open-data.pref.hyogo.lg.jp/?page_id=141'"
    >
      <!-- 件.tested = 検査数 -->
      <!--<template v-if="$i18n.locale !== 'ja-basic'" v-slot:additionalNotes>
        <ul :class="$style.GraphDesc">
          <li>
            {{ $t('（注）同一の対象者について複数の検体を検査する場合あり') }}
          </li>
          <li>
            {{
              $t(
                '（注）速報値として公開するものであり、後日確定データとして修正される場合あり'
              )
            }}
          </li>
        </ul>
        <ol :class="$style.GraphDesc">
          <li>{{ $t('※1: 疑い例・接触者調査') }}</li>
          <li>{{ $t('※2: チャーター便・クルーズ船') }}</li>
        </ol>
      </template>-->
    </time-stacked-bar-chart>
  </v-col>
</template>

<script>
import inspectionsSummary from '@/data/inspections_summary.json'
import TimeStackedBarChart from '@/components/TimeStackedBarChart.vue'

export default {
  components: {
    TimeStackedBarChart
  },
  data() {
    // 検査実施日別状況
    const allInspectionsArray = []
    for (let i = 0; i < inspectionsSummary.data['検査検体数'].length; i++) {
      allInspectionsArray.push(
        inspectionsSummary.data['検査検体数'][i] -
          inspectionsSummary.data['陽性確認'][i]
      )
    }
    const inspectionsGraph = [
      inspectionsSummary.data['陽性確認'],
      allInspectionsArray
    ]

    const inspectionsItems = [this.$t('陽性確認件数'), this.$t('陰性確認件数')]
    const inspectionsLabels = inspectionsSummary.labels
    const inspectionsDataLabels = [
      this.$t('陽性確認件数'),
      this.$t('陰性確認件数')
    ]

    const data = {
      inspectionsSummary,
      inspectionsGraph,
      inspectionsItems,
      inspectionsLabels,
      inspectionsDataLabels
    }
    return data
  }
}
</script>

<style module lang="scss">
.Graph {
  &Desc {
    margin: 0;
    margin-top: 1rem;
    padding-left: 0 !important;
    font-size: 12px;
    color: $gray-3;
    list-style: none;
  }
}
</style>
