<template>

  <div class="container">
  <div class="columns">
    <div class="column col-12">
      <h1>Fields</h1>
    </div>
    <div class="column col-12">
      <div style="width:200px;" class="btn-group btn-group-block">
        <button v-on:click="saveField" class="btn btn-success">Save</button>
        <button v-on:click="addField" class="btn btn-primary">Add field</button>
      </div>
    </div>
    <div class="column col-12">

      <table class="table">
        <thead>
          <tr>
            <th style="width:350px;" colspan="1" rowspan="2">Name</th>
            <th style="width:150px;" colspan="1" rowspan="2">Required?</th>
            <th colspan="2"><div class="text-center">Rules</div></th>
          </tr>
          <tr>
            <th><div class="text-center">Name</div></th>
            <th><div class="text-center">Arguments</div></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(f, kf) in fields" v-bind:key="kf">
            <td>
              <div class="form-group">
                <input class="form-input" type="text" v-model="f.name" placeholder="Name">
              </div>
            </td>
            <td>
              <div class="form-group">
                <label class="form-switch">
                  <input type="checkbox" v-model="f.required">
                  <i class="form-icon"></i>
                </label>
              </div>
            </td>
            <td colspan="2">
              <table class="table">
                <thead>
                  <tr style="background-color: #eee;">
                    <th style="width:250px;"></th>
                    <th></th>
                    <th></th>
                    <th style="width:100px;"><button v-on:click="addRule(kf)" class="btn btn-primary">Add rule</button></th>
                  </tr>
                </thead>
                <tbody>

                  <tr v-for="(r,k) in f.rules" :key="k">
                    <td>
                      <div class="form-group">
                        <label class="form-label">Select a rule</label>
                        <select v-model="r.name" class="form-select" v-on:change="r.args = getRules(r.name); r.obj = {}">
                          <option v-for="(rule, rk) in rules" v-bind:key="rk">{{ rule.name }}</option>
                        </select>
                      </div>
                    </td>
                    <td v-for="arg in r.args" :colspan="r.args.length == 1 ? 2: 0">
                      <div class="form-group">
                        <label class="form-label">{{ arg }}</label>
                        <input v-bind:value="r.arguments[arg]" v-on:input="r.arguments[arg] = $event.target.value" v-on:change="theObj(r, arg, $event.target.value)" class="form-input" :placeholder="arg">
                      </div>
                    </td>
                    <td v-show="r.args.length == 0"></td>
                    <td v-show="r.args.length == 0"></td>
                    <td><button class="btn btn-error btn-sm">Borrar</button></td>
                  </tr>

                </tbody>
              </table>
            </td>
          </tr>
        </tbody>
      </table>

    </div>
  </div>
</div>
</template>

<script>
import rules from './rules';
import 'spectre.css/dist/spectre.css';
import './style.css';
import _ from 'underscore';

export default {
  data(){
    return {
      rules: rules,
      fields: []
    }
  },
  methods: {
    addField(e){
      this.fields.push({name: '', rules: []});
    },
    addRule(kf){
      this.fields[kf].rules.push({name: '', obj: {}, args: {}, arguments: {}});
    },
    saveField(){
      var fields = this.fields, _fields = [];

      _.each(fields, function(field){
        var obj = {name: field.name, required: field.required}, rules = [];
        _.each(field.rules, function(rule){
          var _rule = {};
          _rule['name'] = rule.name;
          _rule['arguments'] = rule.obj;
          rules.push(_rule);
        });
        obj['rules'] = rules;
        _fields.push(obj);
      });

      console.log(_fields);
    },
    theObj(r, _arg, val){
      if(_arg != '__ob__') r.obj[_arg] = val;
    },
    getRules(it){
      var item = rules.findIndex(item => item.name == it);
      return rules[item].arguments;
    },
    countRules(obj){
      return _.size(obj)
    }
  }
}
</script>
