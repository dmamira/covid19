<template>
  <v-col cols="12" md="6" class="DataCard">
    <monitoring-consultation-desk-report-chart
      :title="$t('旧モニタリング指標(7)')"
      title-id="monitoring-number-of-reports-to-covid19-consultation-desk"
      :info-titles="[$t('受診相談窓口における相談件数')]"
      chart-id="monitoring-consultation-desk-report-chart"
      :chart-data="chartData"
      :date="date"
      :labels="labels"
      :data-labels="dataLabels"
      :unit="$t('件.reports')"
      url="https://catalog.data.metro.tokyo.lg.jp/dataset/t000010d0000000070"
    >
      <template v-slot:additionalDescription>
        <ul class="ListStyleNone">
          <li>
            {{
              $t(
                '（注）曜日などによる件数のばらつきにより、日々の結果が変動するため、こうしたばらつきを平準化し全体の傾向を見る趣旨から、過去７日間の移動平均値を折れ線グラフで示す（たとえば、5月7日の移動平均値は、5月1日から5月7日までの実績値を平均したもの）'
              )
            }}
          </li>
          <li>
            {{
              $t(
                '（注）新型コロナ受診相談窓口 （帰国者・接触者電話相談センター）を開設した2月7日から作成'
              )
            }}
          </li>
        </ul>
      </template>
    </monitoring-consultation-desk-report-chart>
  </v-col>
</template>

<script>
import MonitoringConsultationDeskReportChart from '@/components/MonitoringConsultationDeskReportChart.vue'
import Data from '@/data/data.json'

export default {
  components: {
    MonitoringConsultationDeskReportChart
  },
  data() {
    const [
      consulationReportsCount,
      sevendayMoveAverages,
      labels
    ] = Data.querents.data.reduce(
      (res, data) => {
        res[0].push(data['小計'])
        res[1].push(data['７日間平均'])
        res[2].push(data['日付'])
        return res
      },
      [[], [], []]
    )
    const chartData = [consulationReportsCount, sevendayMoveAverages]
    const dataLabels = [this.$t('相談件数'), this.$t('７日間移動平均')]
    const date = Data.querents.date

    return {
      chartData,
      date,
      labels,
      dataLabels
    }
  }
}
</script>
