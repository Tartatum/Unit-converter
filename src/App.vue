<template>
<body style="background-color:#fffdd0;">
  <div>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="container">
        <a class="navbar-brand">Unit converter</a>
      </div>
    </nav>
    <div class="container mt-3">
      <div v-if="ioptions.length > 0"  class="row"> 
        <div class="col-6">
          <div class="card text-center">
            <div class="card-header">
              From
            </div>
            <div class="card-body">
              <p class="card-title"><input v-model="n" placeholder="some number"></p>
              <p class="card-text">
                <select v-model="i">
                  <option v-bind:value="ioption" v-for="ioption in ioptions" :key="ioption.name">{{ioption.name}}</option>
                </select>
              </p>
            </div>
            <div class="card-footer text-muted">
            </div>
          </div>
        </div>
        <div class="col-6">
          <div class="card text-center">
            <div class="card-header">
              To
            </div>
            <div class="card-body">
              <p v-if="isNaN(n)">{{'"'+ n + '"is not a number'}}</p>
              <ul v-else class="list-group list-group-flush">
                <li class="list-group-item" v-for="ioption in ioptions" :key="ioption.name">{{ioption.div * n  / i.div }} {{ioption.name}}</li>
              </ul>
            </div>
            <div class="card-footer text-muted">
              <button class="btn btn-primary" type="button" data-toggle="modal" data-target="#AddForm" aria-expanded="false" aria-controls="AddForm">
                Adding Unit
              </button>
            </div>
          </div>
        </div>
      </div>
      <div class="row" v-else>
        <div class="card">
          <div class="card-header">
            Base unit
          </div>
          <div class="card-body">
            <form>
              <div class="form-group">
                <label for="operation-name" class="col-form-label">Base unit name :<d style="font-size:8px;" > (Be a bit serious)</d></label> 
                <input v-model="base.name" type="text" class="form-control" id="operation-name">
              </div>
            </form>
          </div>
          <div class="card-footer ">
            <button style="float:right;" type="button" @click="defineBase" class="btn btn-primary">Submit</button>
          </div>
        </div>
      </div>
      
      <div class="modal fade" role="dialog" tabindex="-1" aria-hidden="true" id="AddForm">
        <div class="modal-dialog" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="ModalLabel">Add Unit</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
              <form>
                <div class="form-group">
                  <label for="operation-name" class="col-form-label">Unit name :</label>
                  <input v-model="unit.name" type="text" class="form-control" id="operation-name">
                </div>
                <div class="form-group">
                  <label for="operation-div" class="col-form-label">Multiplier <d style="font-size:12px;" > (Must be a number)</d>:</label>
                  <input v-model="unit.div" type="text" class="form-control" id="operation-div">
                </div>

                <div class="input-group">
                  <div class="input-group-append">
                    Example : 1 {{base.name}} = {{value}} {{unit.name}} 
                  </div>
                </div>
              </form>
             
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
              <button type="button" class="btn btn-primary" @click="AddUnit" data-dismiss="modal">Add</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</body>
</template>

<script>
  export default {
    data() {
      return {
        unit : {},
        n: 0,
        base: {div : 1},
        ioptions: [],
        i: {},
        res : {},

      };
    },
    methods: {
      defineBase: function(){
        let b = this.base
        this.ioptions.push(b)
        this.i = {...this.ioptions[0]}
        this.res = {...this.ioptions[0]}
        this.unit = {...this.ioptions[0]}
      },
      AddUnit : function(){
        this.ioptions.push(this.unit)
        this.unit = {...this.ioptions[0]}
      },
      
    },
    computed :{
      operation: function(){
        return isNaN(this.unit.div) ?  "base multiplier" : this.unit.div.toString()
      },
      value: function(){
        let result = 1 * this.unit.div
        return isNaN(result) ?  '##' : result.toString()
      },
    },
  };
</script>
