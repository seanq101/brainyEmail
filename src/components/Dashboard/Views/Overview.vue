<template>
  <div class="content">
    <div class="container-fluid">
      <div class="row">
        <div class="col-xl-6 col-md-6">
          <stats-card>
            <div slot="header" class="icon-warning">
              <i class="nc-icon nc-email-85 text-warning"></i>
            </div>
            <div slot="content">
              <p class="card-category">Total Emails</p>
              <h4 class="card-title">35</h4>
            </div>
            <div slot="footer">
              <i class="fa fa-refresh"></i>Updated now
            </div>
          </stats-card>
        </div>

        <div class="col-xl-6 col-md-6">
          <stats-card>
            <div slot="header" class="icon-success">
              <i class="nc-icon nc-money-coins text-success"></i>
            </div>
            <div slot="content">
              <p class="card-category">Highest Offer Received</p>
              <h4 class="card-title">$1,345</h4>
            </div>
            <div slot="footer">
              <i class="fa fa-calendar-o"></i>Yesterday
            </div>
          </stats-card>
        </div>    
      </div>

      <div class="row">
        <div class="col-md-8">
          <chart-card :chart-data="lineChart.data"
                      :chart-options="lineChart.options"
                      :responsive-options="lineChart.responsiveOptions">
            <template slot="header">
              <h4 class="card-title">Users Behavior</h4>
              <p class="card-category">24 Hours performance</p>
            </template>
            <template slot="footer">
              <div class="legend">
                <i class="fa fa-circle text-info"></i> Open
                <i class="fa fa-circle text-danger"></i> Click
                <i class="fa fa-circle text-warning"></i> Click Second Time
              </div>
              <hr>
              <div class="stats">
                <i class="fa fa-history"></i> Updated 3 minutes ago
              </div>
            </template>
          </chart-card>
        </div>

        <div class="col-md-4">
          <chart-card :chart-data="pieChart.data" chart-type="Pie">
            <template slot="header">
              <h4 class="card-title">Email Statistics</h4>
              <p class="card-category">Bot Performance</p>
            </template>
            <template slot="footer">
              <div class="legend">
                <i class="fa fa-circle text-info"></i> Question Answered By Bot
                <i class="fa fa-circle text-danger"></i> Forwarded To Client
                <i class="fa fa-circle text-warning"></i> Negotiation By Bot
              </div>
              <hr>
              <div class="stats">
                <i class="fa fa-clock-o"></i> Updated Just Now
              </div>
            </template>
          </chart-card>
        </div>
      </div>

      <div class="row">
        

        <div class="col-md-12">
          <card>
            <template slot="header">
              <h5 class="title">Set Bot Rules</h5>
            </template>
            <l-table :data="tableData.data"
                     :columns="tableData.columns">
              <template slot="columns"></template>

              <template slot-scope="{row}">
                <td>
                  <Checkbox v-model="row.checked"></Checkbox>
                </td>
                <td>{{row.title}}</td>
                <td class="td-actions text-right">
                  <button type="button" class="btn-simple btn btn-xs btn-info" v-tooltip.top-center="editTooltip">
                    <i class="fa fa-edit"></i>
                  </button>
                  <button type="button" class="btn-simple btn btn-xs btn-danger" v-tooltip.top-center="deleteTooltip">
                    <i class="fa fa-times"></i>
                  </button>
                </td>
              </template>
            </l-table>
            <div class="footer">
              <hr>
              <div class="stats">
                <i class="fa fa-history"></i> Updated 3 minutes ago
                
                <button type="submit" class="btn btn-info btn-fill float-right margin-btn">Update Rules</button>
                <button v-on:click="showNewRule" class="btn btn-success btn-fill float-right margin-btn">Create a Rule</button>

              </div>
            </div>
          </card>

        </div>
      </div>

      <div class="row">
        <div class="col-md-12">
          <card>
            <template slot="header">
              <h5 class="title">Setup Custom Email Templates</h5>
            </template>
            
            <div class="footer">
              <hr>
              <div class="stats">
                <i class="fa fa-history"></i> Updated 3 minutes ago
                
                <button type="submit" class="btn btn-info btn-fill float-right margin-btn">Update Rules</button>
                <button v-on:click="showNewTemplate" class="btn btn-success btn-fill float-right margin-btn">Create a Rule</button>

              </div>
            </div>
          </card>

        </div>
      </div>



      <modal name="new-template-modal" height="auto" width="75%" scrollable="true" pivotY=".1">
        <div class="header" style="text-align:center">
          <h2>Create a New Rule</h2>
        </div>
        <hr>
        <div class="row">
          <div class="container col-md-6 form-left">
            
            
            <div>
              <input type="checkbox" name="choice-trigger" id="choice-trigger-sender">
              <label for="choice-trigger-sender">Received Email from...</label>
            
              <div class="reveal-if-active">
                <fg-input type="text"
                    id="sender-input" 
                    name="seender-input" 
                    class="require-if-active"
                    label="Add emails below"
                    placeholder="example@email.com, someone@emxbot.com"
                    v-model="newRule.triggerSender">
                </fg-input>
              </div>
            </div>

            <div>
              <input type="checkbox" name="choice-trigger" id="choice-trigger-subject">
              <label for="choice-trigger-subject">Subject line contains...</label>
            
              <div class="reveal-if-active">
                <!-- <label for="subject-input">Why? Cats are weird. Respond.</label> -->
                <fg-input type="text"
                    id="subject-input" 
                    name="subject-input" 
                    class="require-if-active"
                    label="Add keywords below"
                    placeholder="Hello, example, inquiry"
                    v-model="newRule.triggerSubject">
                </fg-input>
              </div>
            </div>


            <div>
              <input type="checkbox" name="choice-trigger" id="choice-trigger-body">
              <label for="choice-trigger-body">Body of the message contains...</label>
            
              <div class="reveal-if-active">
                <!-- <label for="subject-input">Why? Cats are weird. Respond.</label> -->
                <fg-input type="text"
                    id="body-input" 
                    name="body-input" 
                    class="require-if-active"
                    label="Add keywords below"
                    placeholder="Price, offer, details"
                    v-model="newRule.triggerBody">
                </fg-input>
              </div>
            </div>

            <div>
              <input type="checkbox" name="choice-trigger" id="choice-trigger-exceeds">
              <label for="choice-trigger-exceeds">Email exceeds a size of...</label>
            
              <div class="reveal-if-active">
                <!-- <label for="subject-input">Why? Cats are weird. Respond.</label> -->
                <fg-input type="number"
                    id="exceeds-input" 
                    name="exceeds-input" 
                    class="require-if-active"
                    label="Add size limit (in MB)"
                    placeholder="15"
                    v-model="newRule.triggerExceeds">
                </fg-input>
              </div>
            </div>

            <div>
              <input type="checkbox" name="choice-trigger" id="choice-trigger-min">
              <label for="choice-trigger-min">Email must be at least a size of...</label>
            
              <div class="reveal-if-active">
                <!-- <label for="subject-input">Why? Cats are weird. Respond.</label> -->
                <fg-input type="number"
                    id="body-input" 
                    name="body-input" 
                    class="require-if-active"
                    label="Add size requirement (in MB)"
                    placeholder="1"
                    v-model="newRule.triggerMin">
                </fg-input>
              </div>
            </div>

            
            
            


          </div>

          <div class="container col-md-6 form-right">
            
            


          </div>
        </div>

        <hr>

            <div class = "flex-row d-flex justify-content-center">
              <div class="col-md-6">
              
            
                <!-- <label for="subject-input">Why? Cats are weird. Respond.</label> -->
                <fg-input type="text"
                    id="forward-input" 
                    name="forward-input" 
                    class="require-if-active"
                    label="Name of Custom Rule"
                    placeholder="Do the hard work for me"
                    style="text-align:center"
                    v-model="newRule.nameOfRule">
                </fg-input>
              </div>
            </div>

        <hr>
        <div class="bottomButtons">
          <button class="btn btn-info btn-fill float-right margin-btn" @click.prevent="submitNewRule">Save</button>
          <button v-on:click="hideNewRule" class="btn btn-default btn-fill left-right margin-btn">Cancel</button>
          
        </div>

        
        
          
      </modal>















      <modal name="new-rule-modal" height="auto" width="75%" scrollable="true" pivotY=".1">
        <div class="header" style="text-align:center">
          <h2>Create a New Rule</h2>
        </div>
        <hr>
        <div class="row">
          <div class="container col-md-6 form-left">
            
            
            <div>
              <input type="checkbox" name="choice-trigger" id="choice-trigger-sender">
              <label for="choice-trigger-sender">Received Email from...</label>
            
              <div class="reveal-if-active">
                <fg-input type="text"
                    id="sender-input" 
                    name="seender-input" 
                    class="require-if-active"
                    label="Add emails below"
                    placeholder="example@email.com, someone@emxbot.com"
                    v-model="newRule.triggerSender">
                </fg-input>
              </div>
            </div>

            <div>
              <input type="checkbox" name="choice-trigger" id="choice-trigger-subject">
              <label for="choice-trigger-subject">Subject line contains...</label>
            
              <div class="reveal-if-active">
                <!-- <label for="subject-input">Why? Cats are weird. Respond.</label> -->
                <fg-input type="text"
                    id="subject-input" 
                    name="subject-input" 
                    class="require-if-active"
                    label="Add keywords below"
                    placeholder="Hello, example, inquiry"
                    v-model="newRule.triggerSubject">
                </fg-input>
              </div>
            </div>


            <div>
              <input type="checkbox" name="choice-trigger" id="choice-trigger-body">
              <label for="choice-trigger-body">Body of the message contains...</label>
            
              <div class="reveal-if-active">
                <!-- <label for="subject-input">Why? Cats are weird. Respond.</label> -->
                <fg-input type="text"
                    id="body-input" 
                    name="body-input" 
                    class="require-if-active"
                    label="Add keywords below"
                    placeholder="Price, offer, details"
                    v-model="newRule.triggerBody">
                </fg-input>
              </div>
            </div>

            <div>
              <input type="checkbox" name="choice-trigger" id="choice-trigger-exceeds">
              <label for="choice-trigger-exceeds">Email exceeds a size of...</label>
            
              <div class="reveal-if-active">
                <!-- <label for="subject-input">Why? Cats are weird. Respond.</label> -->
                <fg-input type="number"
                    id="exceeds-input" 
                    name="exceeds-input" 
                    class="require-if-active"
                    label="Add size limit (in MB)"
                    placeholder="15"
                    v-model="newRule.triggerExceeds">
                </fg-input>
              </div>
            </div>

            <div>
              <input type="checkbox" name="choice-trigger" id="choice-trigger-min">
              <label for="choice-trigger-min">Email must be at least a size of...</label>
            
              <div class="reveal-if-active">
                <!-- <label for="subject-input">Why? Cats are weird. Respond.</label> -->
                <fg-input type="number"
                    id="body-input" 
                    name="body-input" 
                    class="require-if-active"
                    label="Add size requirement (in MB)"
                    placeholder="1"
                    v-model="newRule.triggerMin">
                </fg-input>
              </div>
            </div>

            
            
            


          </div>

          <div class="container col-md-6 form-right">
            
            <div>
              <input type="checkbox" name="choice-action" id="choice-action-send">
              <label for="choice-action-send">Also CC autmoted reply to...</label>
            
              <div class="reveal-if-active">
                <!-- <label for="subject-input">Why? Cats are weird. Respond.</label> -->
                <fg-input type="text"
                    id="send-input" 
                    name="send-input" 
                    class="require-if-active"
                    label="Add emails below"
                    placeholder="example@email.com, someone@emxbot.com"
                    v-model="newRule.actionSender">
                </fg-input>
              </div>
            </div>

            <div>
              <input type="checkbox" name="choice-action" id="choice-action-reply">
              <label for="choice-action-reply">Reply with custom text...</label>
            
              <div class="reveal-if-active">
                <!-- <label for="subject-input">Why? Cats are weird. Respond.</label> -->
                <fg-input type="text"
                    id="reply-input" 
                    name="reply-input" 
                    class="require-if-active"
                    label="Add custom text that will be included in your response"
                    placeholder="Thank you for your interest in my product!"
                    v-model="newRule.actionReply">
                </fg-input>
              </div>
            </div>


            <div>
              <input type="checkbox" name="choice-action" id="choice-action-forward">
              <label for="choice-action-forward">Forward incoming email to...</label>
            
              <div class="reveal-if-active">
                <!-- <label for="subject-input">Why? Cats are weird. Respond.</label> -->
                <fg-input type="text"
                    id="forward-input" 
                    name="forward-input" 
                    class="require-if-active"
                    label="Add emails below"
                    placeholder="example@email.com, someone@emxbot.com"
                    v-model="newRule.actionForward">
                </fg-input>
              </div>
            </div>

            <div>
              <input type="checkbox" name="choice-action" id="choice-action-discard" v-model="newRule.actionDiscard">
              <label for="choice-action-forward">Discard incoming email</label>
            
              
            </div>



          </div>
        </div>

        <hr>

            <div class = "flex-row d-flex justify-content-center">
              <div class="col-md-6">
              
            
                <!-- <label for="subject-input">Why? Cats are weird. Respond.</label> -->
                <fg-input type="text"
                    id="forward-input" 
                    name="forward-input" 
                    class="require-if-active"
                    label="Name of Custom Rule"
                    placeholder="Do the hard work for me"
                    style="text-align:center"
                    v-model="newRule.nameOfRule">
                </fg-input>
              </div>
            </div>

        <hr>
        <div class="bottomButtons">
          <button class="btn btn-info btn-fill float-right margin-btn" @click.prevent="submitNewRule">Save</button>
          <button v-on:click="hideNewRule" class="btn btn-default btn-fill left-right margin-btn">Cancel</button>
          
        </div>

        
        
          
      </modal>

      <div class="row">
        <div class="col-md-12">
          <div class="card">
            <div class="card-header">
              View All Past Emails
            </div>
            <div class="card-body">
              <button class="accordion">Email 1</button>
              <div class="panel">
                <p>Lorem ipsum...</p>
              </div>

              <button class="accordion">Email 2</button>
              <div class="panel">
                <p>Lorem ipsum...</p>
              </div>

              <button class="accordion">Email 3</button>
              <div class="panel">
                <p>Lorem ipsum...</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
  import ChartCard from 'src/components/UIComponents/Cards/ChartCard.vue'
  import StatsCard from 'src/components/UIComponents/Cards/StatsCard.vue'
  import Card from 'src/components/UIComponents/Cards/Card.vue'
  import LTable from 'src/components/UIComponents/Table.vue'
  import Checkbox from 'src/components/UIComponents/Inputs/Checkbox.vue'

  export default {
    components: {
      Checkbox,
      Card,
      LTable,
      ChartCard,
      StatsCard
    },
    mounted () {
      var acc = document.getElementsByClassName('accordion')
      console.log(acc)
      var i
      for (i = 0; i < acc.length; i++) {
        acc[i].addEventListener('click', function () {
          this.classList.toggle('active')
          var panel = this.nextElementSibling
          if (panel.style.maxHeight) {
            panel.style.maxHeight = null
          } else {
            panel.style.maxHeight = panel.scrollHeight + 'px'
          }
        })
      }
    },
    data () {
      return {
        editTooltip: 'Edit Task',
        deleteTooltip: 'Remove',
        pieChart: {
          data: {
            labels: ['40%', '20%', '40%'],
            series: [40, 20, 40]
          }
        },
        lineChart: {
          data: {
            labels: ['9:00AM', '12:00AM', '3:00PM', '6:00PM', '9:00PM', '12:00PM', '3:00AM', '6:00AM'],
            series: [
              [287, 385, 490, 492, 554, 586, 698, 695],
              [67, 152, 143, 240, 287, 335, 435, 437],
              [23, 113, 67, 108, 190, 239, 307, 308]
            ]
          },
          options: {
            low: 0,
            high: 800,
            showArea: false,
            height: '245px',
            axisX: {
              showGrid: false
            },
            lineSmooth: true,
            showLine: true,
            showPoint: true,
            fullWidth: true,
            chartPadding: {
              right: 50
            }
          },
          responsiveOptions: [
            ['screen and (max-width: 640px)', {
              axisX: {
                labelInterpolationFnc (value) {
                  return value[0]
                }
              }
            }]
          ]
        },
        barChart: {
          data: {
            labels: ['Jan', 'Feb', 'Mar', 'Apr', 'Mai', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'],
            series: [
              [542, 443, 320, 780, 553, 453, 326, 434, 568, 610, 756, 895],
              [412, 243, 280, 580, 453, 353, 300, 364, 368, 410, 636, 695]
            ]
          },
          options: {
            seriesBarDistance: 10,
            axisX: {
              showGrid: false
            },
            height: '245px'
          },
          responsiveOptions: [
            ['screen and (max-width: 640px)', {
              seriesBarDistance: 5,
              axisX: {
                labelInterpolationFnc (value) {
                  return value[0]
                }
              }
            }]
          ]
        },
        tableData: {
          data: [
            {title: 'Enable your custom Email Bot', checked: true},
            {title: 'Allow Bot to perform basic negotiations"', checked: false},
            {title: 'Allow Bot to block incoming spam', checked: true},
            {title: 'Forward emails to you when the bot cannot answer a Customer', checked: true},
            {title: 'Other Rule', checked: false},
            {title: 'Another Rule', checked: false}
          ]
        },
        newRule: {
          triggerSender: '',
          triggerSubject: '',
          triggerBody: '',
          triggerExceeds: 100,
          triggerMin: 0,
          actionSender: '',
          actionReply: '',
          actionForward: '',
          actionDiscard: false,
          nameOfRule: ''
        }
      }
    },
    methods: {
      showNewRule () {
        this.$modal.show('new-rule-modal')
      },
      hideNewRule () {
        this.$modal.hide('new-rule-modal')
      },
      submitNewRule () {
        alert('Your new rule: ' + JSON.stringify(this.newRule))
        this.hideNewRule()
      },
      showNewTemplate () {
        this.$modal.show('new-template-modal')
      },
      hideNewTemplate () {
        this.$modal.hide('new-template-modal')
      }
    }
  }

</script>

<style>
  /* Style the buttons that are used to open and close the accordion panel */
  .accordion {
      background-color: #eee;
      border-radius:0 calc(0.25rem - 1px) calc(0.25rem - 1px) 0;
      color: #444;
      cursor: pointer;
      padding: 18px;
      width: 100%;
      text-align: left;
      border: none;
      outline: none;
      transition: 0.4s;
  }

  /* Add a background color to the button if it is clicked on (add the .active class with JS), and when you move the mouse over it (hover) */
  .accordion:hover {
      background-color: #ccc;
  }

  .accordion:focus {
      outline:none;
  }

  /* Style the accordion panel. Note: hidden by default */
  .panel {
    padding: 0 18px;
    background-color: white;
    max-height: 0;
    overflow: hidden;
    transition: max-height 0.2s ease-out;
  }
  .margin-btn{
    margin:5px;
  }
  .reveal-if-active {
    opacity: 0;
    max-height: 0;
    overflow: hidden;
    transform: scale(0.8);
    transition: 0.5s;
  }
  input[type="checkbox"]:checked ~ .reveal-if-active {
    opacity: 1;
    max-height: 100px; /* little bit of a magic number :( */
    overflow: visible;
  }

  .form-left{
    padding-left:10%;
  }
  .form-right{
    padding-left:10%;
  }

</style>
