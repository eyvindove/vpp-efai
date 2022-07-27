<script setup>
import uniContent from '@/assets/data/unit_content.json'
import geneDistributionContent from '@/assets/data/table.json'
import vGeneContent from '@/assets/data/v_gene.json'

import imgHat from '@/assets/hat_icon.png'
import imgSkin from '@/assets/非黑色素瘤皮膚癌.png'
import imgSkin2 from '@/assets/非黑色素瘤皮膚癌_2.png'
import imgUnitBackgroundImage from '@/assets/unit_bg_img.jpg'

const pageTitle = '非黑色素瘤皮膚癌'
const annotationText = '本比例數值源自國際期刊，提供給您參考，先天基因無法改變，後天生活習慣由您做主。'

const section1 = uniContent.section[0]
const section2 = uniContent.section[1]
const geneDistributionTableData = geneDistributionContent
const geneList = vGeneContent.gene_list
const geneAnalysisTable = vGeneContent.clinvar_table.item_list

geneList.length = 5

const downloadHtml = () => {
  const htmlRowData = document.querySelector('html').innerHTML
  const blob = new Blob([htmlRowData], { type: 'text/plain' })

  const anchorElement = document.createElement('a')
  anchorElement.href = URL.createObjectURL(blob)
  anchorElement.download = 'html-structure.html'
  anchorElement.click()
}
</script>

<template>
  <div class="container">
    <div class="download">
      <button class="download-button" @click="downloadHtml">download</button>
    </div>

    <div class="content">
      <img class="page__bg" :src="imgUnitBackgroundImage" alt="">

      <div class="page page-with-background">
        <div class="section--2col float-left">
          <div class="page__title">{{ pageTitle }}</div>
          <div class="section__subject">{{ section1.subject }}</div>
          <div class="section__subject-eng">{{ section1.subject_eng }}</div>

          <div class="section__list section-margin-bottom">
            <div class="section__list-item"
              v-for="item in section1.content"
              :key="item"
            >
              <img class="section__list-item__icon" :src="imgHat" alt="hat icon" width="100" height="100">
              <div class="section__list-item__text">{{ item }}</div>
            </div>
          </div>
        </div>

        <div class="section--2col">
          <div class="section__subject float-left">先天基因與後天環境影響比例</div>

          <div class="percentage__annotation float-right text-dark-gray">{{ annotationText }}</div>
          <div class="percentage">
            <span class="percentage--gene">43%</span>
            <span class="percentage--env">57%</span>
          </div>
          <img class="percentage__image" :src="imgSkin" alt="非黑色素瘤皮膚癌圖片">
          <div class="percentage__name">
            <span class="percentage__name--gene">基因</span>
            <span class="percentage__name--env">環境</span>
          </div>
        </div>

        <div class="risk-block">
          <div class="section__subject">風險評估</div>
          <img class="risk__image" :src="imgSkin2" alt="非黑色素瘤皮膚癌2圖片">
        </div>

        <div class="risk-table">
          <table>
            <tbody>
              <tr>
                <td colspan="3">基因位點數</td>
              </tr>
              <tr>
                <td></td>
                <td>您的分佈</td>
                <td>台灣人平均值</td>
              </tr>
              <tr class="tr-higher">
                <td>風險高</td>
                <td>{{ geneDistributionTableData.usr.high }}</td>
                <td>{{ geneDistributionTableData.avg.high }}</td>
              </tr>
              <tr class="tr-higher">
                <td>風險低</td>
                <td>{{ geneDistributionTableData.usr.low }}</td>
                <td>{{ geneDistributionTableData.avg.low }}</td>
              </tr>
              <tr class="tr-higher">
                <td>無變異</td>
                <td>{{ geneDistributionTableData.usr.norm }}</td>
                <td>{{ geneDistributionTableData.avg.norm }}</td>
              </tr>
              <tr class="tr-higher">
                <td>總數</td>
                <td>{{ geneDistributionTableData.usr.sum }}</td>
                <td>{{ geneDistributionTableData.avg.sum }}</td>
              </tr>
            </tbody>
          </table>
        </div>

        <div class="section section-margin-bottom section-padding bg-gray">
          <div class="section__subject">{{ section2.subject }}</div>
          <div class="section__list">
            <div class="section__list-item text-dark-gray"
              v-for="item in section2.content"
              :key="item"
            >
              <img class="section__list-item__icon" :src="imgHat" alt="hat icon" width="100" height="100">
              <div class="section__list-item__text">{{ item }}</div>
            </div>
          </div>
        </div>

        <div class="section">
          <div class="section__subject">重要基因說明與風險</div>

          <table class="risk-explain-table">
            <thead>
              <tr>
                <th class="col1">基因名稱</th>
                <th class="col2">基因說明</th>
                <th class="col3">風險(顯著)型</th>
                <th class="col4">帶因型</th>
                <th class="col5">一般型</th>
                <th class="col6">風險值</th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="item in geneList"
                :key="item.gene_name"
              >
                <td class="col1">{{ item.gene_name }}</td>
                <td class="col2">{{ item.gene_explain }}</td>
                <td
                  class="col3"
                  :class="{ 'text-red': item.high === 'GG', 'text-green': item.high !=='GG' }"
                >
                  {{ item.high }}
                </td>
                <td class="col4 text-gray">{{ item.general }}</td>
                <td class="col5 text-gray">{{ item.low }}</td>
                <td
                  class="col6 text-white"
                  :class="{ 'bg-red': Number(item.risk) >= 1, 'bg-green': Number(item.risk) < 1 }"
                >
                  <div>{{ item.risk }}</div>
                  <div>(OR)</div>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

      <div class="page">
        <div class="page__title">{{ pageTitle }}</div>

        <div class="section">
          <div class="section__subject">重要基因位點分析</div>

          <table class="risk-analysis-table">
            <thead>
              <tr>
                <th class="col1">基因名稱</th>
                <th class="col2">檢測位點數</th>
                <th class="col3">風險/帶因型(數量)</th>
                <th class="col4">基因說明</th>
                <th class="col5">判定</th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="item in geneAnalysisTable"
                :key="item.gene_info.name"
              >
                <td class="col1">{{ item.gene_info.name }}</td>
                <td class="col2">{{ item.gene_info.total }}</td>
                <td class="col3">
                  <span>{{  item.gene_info.high }}</span>
                  /
                  <span>{{  item.gene_info.mid }}</span>
                </td>
                <td class="col4">{{ item.gene_info.explain }}</td>
                <td
                  class="col5"
                  :class="{
                    'text-green': item.gene_info.clinvar_class === '正常',
                    'text-red': item.gene_info.clinvar_class !== '正常',
                  }"
                >
                  {{ item.gene_info.clinvar_class }}
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

    </div>
  </div>
</template>

<style scoped lang="scss">
@import './styles/index.scss';
</style>
