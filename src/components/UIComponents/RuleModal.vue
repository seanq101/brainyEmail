<template>
  <div>
        <div class="header" style="text-align:center">
          <h2>Create a New Rule</h2>
        </div>
        <hr>
        <div class="container">
          
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
        <hr>
        <div class="bottomButtons">
          <button v-on:click="hide" class="btn btn-info btn-fill float-left margin-btn">Save</button>
          <button v-on:click="hide" class="btn btn-default btn-fill float-right margin-btn">Cancel</button>
          
        </div>
  </div>
</template>


<script>
  export default {
    name: 'modal'
  }
</script>


<style>
</style>
