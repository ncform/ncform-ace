<template>
  <div>
    <ncform
      :form-schema="formSchema"
      form-name="your-form-name"
      v-model="formSchema.value"
      @submit="submit()"
    ></ncform>
    <hr>
    <el-button @click="submit()">Submit</el-button>
  </div>
</template>

<script>
import NcformAce from "./components";

export default {
  created() {
    this.$ncformAddWidget({ name: "ncform-ace", widget: NcformAce });
  },
  data() {
    return {
      formSchema: {
        type: "object",
        properties: {
          demo: {
            type: "string",
            ui: {
              widget: "ncform-ace",
              widgetConfig: {
                height: '300px'
              }
            }
          }
        },
        value: {
          demo: JSON.stringify({
            type: "object",
            properties: {
              keyword: {
                type: "string"
              }
            },
            ui: {
              widgetConfig: {
                layout: "h"
              }
            }
          })
        }
      }
    };
  },
  methods: {
    submit() {
      this.$ncformValidate("your-form-name").then(data => {
        if (data.result) {
          console.log(this.$data.formSchema.value);
        }
      });
    }
  }
};
</script>
