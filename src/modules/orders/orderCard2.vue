<template>
  <div style="min-height: 653px">
    <div class="card card2" v-if="Object.keys(data).length > 0 && (restaurant.length > 0 || deliStore.length > 0)" style="overflow: hidden;position: relative;width: 100%;">
      <div class="card-header card2Header">
        <div class="row card2HeaderEl">
          <div class="col-sm-12 d-flex justify-content-between headerActions" v-if="data.order_status.toLowerCase() === 'processing' || data.order_status.toLowerCase() === 'delivering' || data.order_status.toLowerCase() === 'complete'">
            <div class="col-sm-3 text-center divAsButton">
              <b>{{data.id}}</b>
            </div>
            <div class="col-sm-3 p-0 text-center divAsButton" :style="'color: #0064B1;'">
              <b class='font-weight-normal'>DELIVERY TIME: {{data.delivery_time}}</b>
            </div>
            <div class="col-sm-3 text-center divAsButton">
              <b v-if="data.order_status.toLowerCase() === 'processing' || data.order_status.toLowerCase() === 'delivering' || data.order_status.toLowerCase() === 'complete'" :style="'color: #FFBF51; text-transform: uppercase;'">{{data.order_status}}</b>
            </div>
            <div class="col-sm-3 text-center divAsButton" :style="'color: #0064B1;'" @click="viewReceipt(data)">
              <b> PRINT ORDER </b>
            </div>
          </div>
          <div v-else-if="data.order_status.toLowerCase() === 'pending'" class="col-sm-12 text-center">
            <b style="color: #0064B1;"> NEW ORDER REQUEST AT 12:42 </b> 
          </div>
        </div>
      </div>
      <div class="card-body p-0" >
        <div class="col-sm-12">
        <div class="row">
          <div class="col-sm-7 p-0">
            <div class="col-sm-12">
              <div class="row">
                <div v-if="data.order_status.toLowerCase() === 'pending'" class="col-sm-12 pt-3 pb-3 pendingCustomerInformation">
                  <div class="row">
                    <div class="col-sm-5 p-0 pl-3">
                      <div>
                        <b> Customer Information </b>
                      </div>
                      <div>
                        <b class="font-weight-normal"> {{
                          ((data.customer.first_name !== '' || data.customer.first_name !== NULL) ? data.customer.first_name  : '--' )
                            + ' ' + 
                          ((data.customer.last_name !== '' || data.customer.last_name !== NULL) ? data.customer.last_name : '--')}} </b>
                      </div>
                      <div>
                        <b style="color: #0064B1;"> Contact number </b>
                      </div>
                    </div>
                    <div class="col-sm-7 pr-0">
                      <div>
                        <b>
                          Deliver to:
                        </b>
                      </div>
                      <div v-if="data.shipping_address !== null && data.shipping_address !== '' && data.shipping_address !== undefined">
                        <b class="font-weight-normal" v-if="data.shipping_address.address1 !== '' && data.shipping_address.address1 !== null && data.shipping_address.address1 !== undefined">
                          {{data.shipping_address.address1}}
                        </b>
                        <b class="font-weight-normal" v-else-if="data.shipping_address.address2 !== '' && data.shipping_address.address2 !== null && data.shipping_address.address2 !== undefined">
                          {{data.shipping_address.address2}}
                        </b>
                        <i v-else> No shipment address </i>
                      </div>
                      <i v-else> No shipment address </i>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="orderInformation col-sm-12" :style="(data.order_status.toLowerCase() !== 'pending') ? 'height: 513px; margin-top: -1px;' : 'height: 488px;'">
              <div class="mt-3">
                <b :style="'color: #0064B1'"> Restaurant Items: </b>
              </div>
              <div v-for="(el, ndx) in restaurant" :key="'restaurant' + ndx" class="mt-2 pr-5">
                <div class="d-flex justify-content-between">
                  <b :style="'color: #E07700'"> {{el.product.name}} </b>
                  <b class="font-weight-normal"> {{data.customer_currency_code}} {{el.product.price}} </b>
                </div>
                <div class="col-sm-12" :style="'color: #E07700'">
                  + {{el.product.short_description}}
                </div>
              </div>
              <div class="mt-3">
                <b :style="'color: #0064B1'"> Deli product Items: </b>
              </div>
              <div v-for="(el, ndx) in deliStore" :key="'deli' + ndx" class="mt-2 pr-5">
                <div class="d-flex justify-content-between">
                  <b :style="'color: #E07700'"> {{el.product.name}} </b>
                  <b class="font-weight-normal"> {{data.customer_currency_code}} {{el.product.price}} </b>
                </div>
                <div class="col-sm-12" :style="'color: #E07700'">
                  + {{el.product.short_description}}
                </div>
              </div>
              <div class="mt-3">
                <b :style="'color: #0064B1'"> Other add-ons and requests: </b>
              </div>
              <div class="mt-2">
                <b :style="'color: #E07700'"> Contactless delivery </b>
              </div>
              <div class="mt-2">
                <b :style="'color: #E07700'"> Add cutlery </b>
              </div>
            </div>
          </div>
          <div class="col-sm-5 customerInformation" :style="data.order_status.toLowerCase() === 'pending' ? 'height: 581px !important; padding-bottom: 50px !important; overflow-y: scroll !important;': 'height: 513px; padding-left: 15px; padding-right: 15px;'">
            <div v-if="data.order_status.toLowerCase() === 'processing' || data.order_status.toLowerCase() === 'delivering' || data.order_status.toLowerCase() === 'complete'">
              <div class="mt-3">
                <b class="head"> Customer Information </b>
              </div>
              <div class="mt-2">
                <b class="font-weight-normal"> {{ (data.customer.first_name + ' ' + data.customer.last_name).toUpperCase() }} </b>
              </div>
              <div class="mt-1">
                <b class="text-primary"> Contact customer </b>
              </div>
              <br>
              <div class="mt-5">
                <b class="head"> Address: </b>
              </div>
              <div v-if="data.shipping_address !== null && data.shipping_address !== '' && data.shipping_address !== undefined">
                <b class="font-weight-normal" v-if="data.shipping_address.address1 !== '' && data.shipping_address.address1 !== null && data.shipping_address.address1 !== undefined">
                  {{data.shipping_address.address1}}
                </b>
                <b class="font-weight-normal" v-else-if="data.shipping_address.address2 !== '' && data.shipping_address.address2 !== null && data.shipping_address.address2 !== undefined">
                  {{data.shipping_address.address2}}
                </b>
                <i v-else> No shipment address </i>
              </div>
              <i v-else> No shipment address </i>
              <div class="row sectionRow">
                <div class="totalSection">
                  <div class="d-flex justify-content-between">
                    <b>
                      TOTAL
                    </b>
                    <b>
                      {{data.customer_currency_code}} {{data.order_total}}
                    </b>
                  </div>
                </div>
              </div>
            </div>
            <div v-else-if="data.order_status.toLowerCase() === 'pending'">
              <center>
                <div class="mt-4 mb-4" style="width: 100%">
                  <b style="color: #00AF5F;"> ACCEPT FOR: </b>
                </div>
                <div class="mt-2" v-for="(el, ndx) in times" :key="'time' + ndx">
                  <button class="squareButtonCommon" @click="focusIndex = ndx" :style="(focusIndex != ndx) ? unfocusStyle : focusStyle"> {{el.start_time + ' - ' + el.end_time}} </button>
                </div>
                <div class="mt-5">
                  <button class="roundButtonCommon font-weight-bold" :style="focusStyle" @click="accept(data.id)"> ACCEPT </button>
                </div>
                <div class="mt-3">
                  <button class="roundButtonCommon font-weight-normal" :style="dangerUnfocusStyle" data-toggle="modal" data-target="#reasonModal"> Unable to fulfill order </button>
                </div>
              </center>
            </div>
          </div>
        </div>
        </div>
      </div>
      <div class="card-footer card2Footer" v-if="data.order_status.toLowerCase() === 'processing' || data.order_status.toLowerCase() === 'delivering' || data.order_status.toLowerCase() === 'complete'">
        <div class="col-sm-12 p-0">
          <div class="row">
            <div class="progressbtnWidth p-1" v-for="(el, ndx) in progressButtons" :key="ndx">
              <!-- <div class="" style="width: 100%;"> -->
                <div class="col-sm-12 p-1 progressbtn">

                  <div class="p-0">
                    <b>{{el.text}}</b>
                  </div>

                  <div class=" p-0 Progress switch" v-if="data.order_status.toLowerCase() === 'processing' || data.order_status.toLowerCase() === 'delivering' || data.order_status.toLowerCase() === 'complete'">
                    <div v-if="el.status.toLowerCase() === 'yes'" :class="el.status === 'no'? 'switchAllowed' : 'switchNotAllowed'"  @click="el.status === 'no' ? updateOrderStatus(data.id, el.stats) : ''">
                      <i class="fas fa-check-circle switchIcon" :style="'margin-left: -6.5px; color: #7ABC87;'" disbled></i>
                      <b style="position: absolute; margin-top: -2px; top: 50%; transform: translate(0, -50%)"> {{el.status}} </b>
                    </div>
                    <div v-else-if="el.status.toLowerCase() === 'no'" @click="el.disabled === true ? '' : updateOrderStatus(data.id, el.stats, data.order_status)" :class="el.disabled === false ? 'switchAllowed' : 'switchNotAllowed'">
                      <b style="position: absolute; right: 0; margin-right: 2px; margin-top: -2px; top: 50%; transform: translate(0, -50%)"> {{el.status}} </b>
                      <i class="fas fa-circle switchIcon" :style="emptyCircle"></i>
                    </div>
                  </div>
                  <!-- <div class="col-sm-4 p-0 Progress switch" v-if="data.order_status.toLowerCase() === 'delivered'">
                    <div v-if="el.status.toLowerCase() === 'yes'">
                      <i class="fas fa-check-circle switchIcon" :style="'margin-left: -6.5px; color: #7ABC87;'"></i>
                      <b style="position: absolute; margin-top: -2px; top: 50%; transform: translate(0, -50%)"> {{el.status}} </b>
                    </div>
                    <div v-else-if="el.status.toLowerCase() === 'no'">
                      <b style="position: absolute; right: 0; margin-right: 2px; margin-top: -2px; top: 50%; transform: translate(0, -50%)"> {{el.status}} </b>
                      <i class="fas fa-circle switchIcon" :style="emptyCircle"></i>
                    </div>
                  </div> -->

                </div>
              <!-- </div> -->
            </div>
            <div class="progressbtnWidth cancelWrapper" :hidden="data.order_status.toLowerCase() === 'complete'">
              <div class="cancel" @click="updateOrderStatus(data.id, 'cancel')">
                <b>
                  CANCEL ORDER
                </b>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="modal fade bd-example-modal-lg reasonModalContainer" id="reasonModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-lg reasonModal" role="document">
          <div class="modal-content">
            <div class="modal-header modalHeader">
              <b class="font-weight-bold reasonHeader"> UNABLE TO FULFILL ORDER </b>
              <button type="button" ref="xButton" class="xButton close" data-dismiss="modal" aria-label="Close">
                <b>&times;</b>
              </button>
            </div>
            <div class="modal-body pt-2">
              <div class="col-sm-12">
                <div class="row">
                  <div class="col-sm-4" v-for="(el, ndx) in rejectReasons" :key="'reason' + ndx">
                    <div class="reasonCard">
                      <div class="mb-2">
                        <input type="radio" class="radio" :name="'reason'" v-model="rejectionReason"  :value="el.id">
                      </div>
                      <div>
                        <b>{{el.text}}</b>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="modal-footer modalFooter">
              <button class="roundButtonCommon rejectButton font-weight-bold" @click="reject(data.id)"> REJECT ORDER </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-else class="notFoundContainer">
      <div class="notFound text-center">
        <img :src="require('src/assets/img/logo_white.png')" style="width: 20%; height: auto;"/>
        <div class="mt-2">
          <b class="font-weight-normal"> 404 | No data yet</b>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import pdfFonts from 'pdfmake/build/vfs_fonts'
import PDFTemplate from 'pdfmake'
import TemplatePdf from './PdfTemplate.js'
import _ from 'lodash'
export default {
  props: {
    data: {
      default: {},
      type: Object
    },
    restaurant: {
      default: [],
      type: Array
    },
    deliStore: {
      default: [],
      type: Array
    },
    times: {
      default: [],
      type: Array
    }
  },
  mounted(){
    const {vfs} = pdfFonts.pdfMake
    PDFTemplate.vfs = vfs
    console.log('[DATA]', this.data)
    this.getImage()
    if(!_.isEmpty(this.data)){
      if(this.data.order_status.toLowerCase() === 'processing'){
        this.progressButtons[0].status = 'yes'
        this.progressButtons[1].status = 'no'
        this.progressButtons[2].status = 'no'
      }else if(this.data.order_status.toLowerCase() === 'delivering'){
        this.progressButtons[0].disabled = true
        this.progressButtons[0].status = 'yes'
        this.progressButtons[1].status = 'yes'
        this.progressButtons[2].status = 'no'
      }else{
        this.progressButtons[0].status = 'yes'
        this.progressButtons[1].status = 'yes'
        this.progressButtons[2].status = 'yes'
      }
    }
    // if(!_.isEmpty(this.data)){
    //   this.data.order_items.forEach(el => {
    //     if(el.product.category_type === 3){
    //       this.restaurant.push(el)
    //     }else if(el.product.category_type === 1){
    //       this.deliStore.push(el)
    //     }
    //   })
    // }
  },
  data() {
    return {
      emptyCircle: 'font-size: 17px; position: absolute; left: 0 !important;',
      progressButtons: [
        {stats: 'processing', text: 'Processing order', status: 'Yes', value: 'processing_order', disabled: false},
        {stats: 'delivery', text: 'Out for delivery', status: 'No', value: 'out_for_delivery', disabled: false},
        {stats: 'complete', text: 'Delivered', status: 'No', value: 'delivered', disabled: false}
      ],
      focusStyle: 'background-color: #00AF5F; color: #FFFFFF; border 1px solid #00AF5F !important;',
      unfocusStyle: 'background-color: #FFFFFF; border: 1px solid #00AF5F; color: #00AF5F; border 1px solid #00AF5F !important;',
      dangerUnfocusStyle: 'background-color: #FFFFFF; border: 1px solid #BE0000; color: #BE0000; border 1px solid #00AF5F !important;',
      rejectReasons: [
        {
          id: 50,
          text: 'Item unavailable'
        },
        {
          id: 60,
          text: 'Too busy to process order'
        },
        {
          id: 70,
          text: 'Too late to take order'
        }
      ],
      focusIndex: 0,
      PdfTemplate: TemplatePdf,
      dataPdf: [],
      dataRes: [],
      dataDel: [],
      rejectionReason: null,
      image: null
    }
  },
  methods: {
    switch(event) {
    },
    getImage() {
      this.APIGetRequest('get_mts_logo_base64', response => {
        this.image = response
      }, error => {
        console.log(error, ' <-=---------- ERROR <-|->')
      })
    },
    viewReceipt(data){
      this.PdfTemplate.getImage(this.image)
      this.dataPdf = data
      this.dataRes = []
      this.dataDel = []
      this.dataPdf.order_items.map(key => {
        if(key.product.category_type === 0){
          this.dataRes.push(key)
          this.PdfTemplate.getData(this.dataRes)
        }else if(key.product.category_type === 1){
          this.dataDel.push(key)
          this.PdfTemplate.getDel(this.dataDel)
        }
      })
      this.PdfTemplate.getItem(data)
      this.PdfTemplate.template()
    },
    accept(id) {
      console.log('order Time: ', this.times[this.focusIndex].id)
      $('#loading').css({'display': 'block'})
      this.APIPutRequest(`update_order_status?orderId=${id}&orderStatusId=20&orderAcceptTimeId=${this.times[this.focusIndex].id}`, {}, response => {
        console.log('Accept order response: ', response)
        $('#loading').css({'display': 'none'})
        this.$emit('orderProcessed', {id: id, process: 'accepted'})
      }, error => {
        console.log('Accepting order error: ', error)
      })
    },
    updateOrderStatus(id, status, currStatus){
      let tempStatus = null
      if(currStatus.toLowerCase() === 'processing' && status === 'complete'){
        this.progressButtons[2].disabled = true
        return
      }
      if(status === 'delivery'){
        this.progressButtons[1].status = 'yes'
        tempStatus = 25
      }else if(status === 'complete'){
        tempStatus = 30
      }else{
        tempStatus = 40
      }
      console.log('status', status, currStatus)
      $('#loading').css({'display': 'block'})
      this.APIPutRequest(`update_order_status?orderId=${id}&orderStatusId=${tempStatus}`, {}, response => {
        console.log('Accept order response: ', response)
        $('#loading').css({'display': 'none'})
        this.$emit('orderProcessed', {id: id, process: status === 'complete' ? 'complete' : 'delivering'})
      }, error => {
        console.log('Accepting order error: ', error)
      })
    },
    reject(id) {
      console.log('reject reason index : ', id)
      $('#loading').css({'display': 'block'})
      this.APIPutRequest(`update_order_status?orderId=${id}&orderStatusId=${this.rejectionReason}`, {}, response => {
        console.log('Reject order response: ', response)
        $('#loading').css({'display': 'none'})
        this.$refs['xButton'].click()
        this.$emit('orderProcessed', {id: id, process: 'reject'})
      }, error => {
        console.log('Accepting order error: ', error)
      })
    },
    updateStatus(e, status) {
      if(status.toLowerCase() === 'out for delivery') {
        console.log('<------- =========== --------->', e)
      }
    }
  },
  watch: {
    restaurant: function(newVal, oldVal) {
      return newVal
    },
    deliStore: function(newVal, oldVal) {
      return newVal
    }
  },
  updated() {
    console.log('[status]', this.data.order_status.toLowerCase())
    if(!_.isEmpty(this.data)){
      if(this.data.order_status.toLowerCase() === 'processing'){
        this.progressButtons[0].status = 'yes'
        this.progressButtons[1].status = 'no'
        this.progressButtons[2].status = 'no'
      }else if(this.data.order_status.toLowerCase() === 'delivering'){
        this.progressButtons[0].disabled = true
        this.progressButtons[0].status = 'yes'
        this.progressButtons[1].status = 'yes'
        this.progressButtons[2].status = 'no'
      }else{
        this.progressButtons[0].status = 'yes'
        this.progressButtons[1].status = 'yes'
        this.progressButtons[2].status = 'yes'
      }
    }
  }
}
</script>
<style scoped>
.notFoundContainer {
  min-height: 581px;
}
.notFound {
  position: absolute;
  top: 50%;
  left: 50%;
  margin-right: -50%;
  transform: translate(-50%, -50%);
  display: inline-block;
}
.divAsButton {
  cursor: pointer;
}
.customerInformation {
  padding-bottom: 50px !important;
  border-left: 1px solid #707070;
  border-top: 1px solid #707070;
  /* border: 1px solid #707070; */
}
.orderInformation {
  border-left: 1px solid #707070;
  border-top: 1px solid #707070;
  /* height: calc(100vh - 378px) !important; */
  overflow-y: scroll;
  padding-bottom: 50px;
}
.card2 {
  border: none;
}
.card2Header {
  display: flex;
  align-items: center;
  border-top: 1px solid #707070;
  border-left: 1px solid #707070;
  /* border: 1px solid #707070; */
  border-radius: 0px;
  min-height: 72px;
  background-color: #FFFFFF;
}
.card2HeaderEl {
  display: flex !important;
  align-items: 'center' !important;
  width: 100%;
}
.card2Footer {
  min-height: 63px;
  background-color: #FFFFFF;
  border-left: 1px solid #707070;
  border-top: 1px solid #707070;
  padding-top: 2px;
  padding-bottom: 2px;
  display: flex;
  justify-content: center !important;
}
.totalSection {
  min-height: 40px;
  padding-left: 15px;
  padding-right: 15px;
  width: 100%;
  border-top: 1px solid #707070;
  display: flex;
  align-items: center;
}
.totalSection div {
  width: 100%;
}
.sectionRow {
  width: 100%;
  position: absolute;
  bottom: 0;
}
.Progress {
  /* display: flex; */
  /* align-items: center; */
  background-color: #FFFFFF; 
}
.progressbtn {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  margin: 1px;
  height: 54px;
  width: 243px;
  padding-left: 10px !important;
  padding-right: 60px !important;
  border-radius: 50px;
  cursor: default;
  border: 1px solid #B2AFAF;
  color: #585858;
}
.progressbtnWidth {
  width: 25%;
}
.switch {
  width: 45px;
  height: 20px;
  /* display: flex;
  justify-content: space-between !important;
  align-items: center; */
  position: absolute;
  right: 0;
  margin-right: 5px;
  border: 1px solid #B2AFAF;
  border-radius: 20px;
  padding-left: 5px !important;
  padding-right: 5px !important;
}
.switchIcon {
  /* margin-top: 1.7px; */
  font-size: 17px;
  position: absolute;
  right: 0;
}
.switchAllowed{
  cursor: pointer;
}
.switchNotAllowed{
  cursor:not-allowed;
}
.cancel {
  display: flex;
  align-items: center;
  justify-content: center !important;
  height: 63px;
  color: #BE0000;
  cursor: pointer;
}
.cancelWrapper {
  display: flex;
  justify-content: center !important;
  align-items: center !important;
}
.squareButtonCommon {
  height: 52px;
  width: 219px;
  padding-left: 20px;
  padding-right: 20px;
  border-radius: 10px;
  border: 0px;
  cursor: pointer;
}
.squareButtonCommon:focus {
  outline: none;
  box-shadow: none;
}
.roundButtonCommon {
  height: 64px;
  width: 219px;
  font-size: 18px;
  padding-left: 20px;
  padding-right: 20px;
  border-radius: 50px;
  cursor: pointer;
  border: 0px;
}
.roundButtonCommon:focus {
  outline: none;
  box-shadow: none;
}
.pendingCustomerInformation {
  border-bottom: 1px solid #707070;
  border-left: 1px solid #707070;
  border-top: 1px solid #707070;
}
.meantime {
  /* align-items: center;
  justify-content: space-between !important; */
}
.xButton {
  position: absolute;
  right: 0;
  top: 0;
  background-color: #FFFFFF;
  color: #FF0045;
  border-radius: 50%;
  width: 27px;
  height: 27px;
  margin: 0 auto;
  padding: 0;
  padding-top: -10px !important;
  padding-bottom: 10px !important;
  display: table-cell;
  vertical-align: middle;
  opacity: 1;
  margin-top: -10px;
  margin-right: -10px;
}
.xButton:after {
  opacity: 0.2 !important;
}
.xButton:focus {
  outline: none;
  box-shadow: none;
}
.modalHeader {
  display: flex;
  text-align: center;
  justify-content: center;
  border: 0px;
  min-height: 92px;
}
.modalFooter {
  display: flex;
  text-align: center;
  justify-content: center;
  border: 0px;
  min-height: 92px;
  padding-bottom: 50px;
  padding-top: 30px;
}
.reasonCard {
  -webkit-box-shadow: 1px 1px 15px 0px rgba(0,0,0,0.19); 
  box-shadow: 1px 1px 15px 0px rgba(0,0,0,0.19);
  min-height: 110px;
  padding: 15px;
  margin-top: 7px;
  display: block;
  text-align: center;
  justify-content: center;
}
.radio {
  width: 19px;
  height: 19px;
  border: 1px solid #0064B1;
}

input[type='radio']:before {
  width: 19px;
  height: 19px;
  border: 1px solid #707070 !important;
}

input[type="radio"]:checked:before {
  background: #0064B1;
}

input[type="radio"]:checked {
  border-color:#0064B1;
}
.rejectButton {
  background-color: #BE0000;
  color: white;
}
.reasonHeader {
  color:  #BE0000;
  font-size: 18px;
}
.reasonModalContainer {
  margin-top: -50px; 
  text-align: center;
  padding: 0!important;
}

.reasonModalContainer:before {
  content: '';
  display: inline-block;
  min-height: 100%;
  vertical-align: middle;
}

.reasonModal {
  display: inline-block;
  text-align: left;
  vertical-align: middle;
}
.orderInformation::-webkit-scrollbar-track
{
	/* -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3); */
	background-color: #F5F5F5;
}
.orderInformation::-webkit-scrollbar
{
  width: 7px;
	background-color: #F5F5F5;
}
.orderInformation::-webkit-scrollbar-thumb
{
	background-color: #707070;
	border: 0px;
  border-radius: 10px;
}

.customerInformation::-webkit-scrollbar-track
{
	/* -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3); */
	background-color: #F5F5F5;
}
.customerInformation::-webkit-scrollbar
{
  width: 7px;
	background-color: #F5F5F5;
}
.customerInformation::-webkit-scrollbar-thumb
{
	background-color: #707070;
	border: 0px;
  border-radius: 10px;
}
@media (max-width: 1450px) {
  .roundButtonCommon {
    width: 85%;
    font-size: 12px;
    padding-left: 5px;
    padding-right: 5px;
  }
  .squareButtonCommon {
    width: 85%;
    font-size: 12px;
    padding-left: 5px;
    padding-right: 5px;
  }
}
@media (max-width: 1300px) {
  .progressbtn {
    font-size: 12px !important;
    width: 100%;
  }
  .progressbtnWidth {
    width: 100%;
  }
}
.headerActions {
  align-items: center;
}
</style>
