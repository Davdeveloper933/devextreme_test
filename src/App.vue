<template>
  <div>
    <DxButton
            text="Создать график"
            type="default"
            @click="showInfo"
    />
    <DxPopup
            :visible="popupVisible"
            :drag-enabled="false"
            :close-on-outside-click="false"
            :show-close-button="false"
            :show-title="true"
            :width="600"
            :height="480"
            container=".dx-viewport"
            title="Создать график"
    >
      <DxPosition
              at="center"
              my="center"
      />
      <DxToolbarItem
              widget="dxButton"
              toolbar="bottom"
              location="after"
              :options="closeButtonOptions"
      />
      <DxToolbarItem
              widget="dxButton"
              toolbar="bottom"
              location="before"
              :options="submitButtonOptions"
      />
        <div class="dx-field">
          <div class="dx-field-label">Выберите тип графика</div>
            <div class="dx-field-value">
        <DxSelectBox
                :items="chartsOptions"
                v-model="chartType"
        />
            </div>
        </div>
        <div class="dx-field" >
        <div class="dx-field-label" v-if="chartType">Выберите источник данных</div>
            <div class="dx-field-value" v-if="chartType">
        <DxSelectBox
                :items="sourceOptions"
                v-model="sourceType"
        />
            </div>
        </div>
    </DxPopup>
    <DxList
            :data-source="chartsList"
            height="100%"
    >
      <template #item>
        <DxPieChart
                id="pie"
                :data-source="pieData"
                palette="Bright"
                title="Area of Countries"
                @point-click="pointClickHandler($event)"
                @legend-click="legendClickHandler($event)"
        >
          <DxSeries
                  argument-field="Группа"
                  value-field="Доля"
          >
            <DxLabel :visible="true">
              <DxConnector
                      :visible="true"
                      :width="1"
              />
            </DxLabel>
          </DxSeries>
          <DxSize :width="500"/>
          <DxExport :enabled="true"/>
        </DxPieChart>
      </template>
    </DxList>
  </div>
</template>

<script>
  import DxButton from 'devextreme-vue/button';
  import {DxPopup,DxToolbarItem,DxPosition} from 'devextreme-vue/popup'
  import {
    DxLabel
  } from 'devextreme-vue/form';
  import DxList from 'devextreme-vue/list';
  import axios from 'axios';
  import DxPieChart, {
    DxSize,
    DxSeries,
    DxConnector,
    DxExport,
  } from 'devextreme-vue/pie-chart';
  import DxSelectBox from 'devextreme-vue/select-box';

  export default {
    components: {
      DxButton,
      DxPopup,
      DxToolbarItem,DxPosition,DxList,
      DxSize,
      DxSeries,
      DxLabel,
      DxConnector,
      DxPieChart,
      DxExport,
      DxSelectBox
    },
    data () {
      return {
        chartType: '',
        sourceType: '',
        pieData: {},
        linearData: {},
        popupVisible: false,
        chartsList: [],
        chartsOptions: [
              'Линейный',
              'Пирог'
          ],
        sourceOptions: [
              'https://run.mocky.io/v3/92a0a266-0321-4ff5-9993-b394d03ceee2',
              'https://run.mocky.io/v3/2699115b-8ced-40c3-8072-b7fa9faf6047'
          ],
        closeButtonOptions: {
          text: 'Закрыть',
          onClick: () => {
            this.showInfo()
          },
        },
        submitButtonOptions: {
          text: 'Создать',
          type: 'default',
          onClick: () => {
            this.addToList()
            this.closeModal()
          },
        },
      }
    },
    created() {
      axios.get('https://run.mocky.io/v3/2699115b-8ced-40c3-8072-b7fa9faf6047')
      .then(response => {
        this.pieData = response.data;
        console.log(this.pieData)
      })
    },
    methods: {
      sayHelloWorld() {
        alert('Hello world!')
      },
      showInfo() {
        this.popupVisible = !this.popupVisible;
      },
      closeModal() {
        this.popupVisible = false;
      },
      addToList() {
          this.chartsList.push({
              chart: this.chartType
          })
      }
    }
  }
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
