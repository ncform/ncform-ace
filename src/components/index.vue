<template>
  <div class="ncform-ace">
    <div class="wrapper">
      <div class="item-wrapper">
        <div ref="ace" :style="{height: mergeConfig.height}"></div>
      </div>

      <div class="split-line">
        <i class="el-icon-arrow-right"></i>
      </div>

      <div class="item-wrapper" :style="{height: mergeConfig.height}">
        <ncform v-if="formSchema" :form-schema="formSchema" :form-name="formName"></ncform>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.ncform-ace {
  .wrapper {
    border: 1px solid #dcdfe6;
    display: flex;
    .item-wrapper {
      flex: 1;
      overflow-y: auto;
    }
    .split-line {
      border: 1px solid #dcdfe6;
      border-radius: 8px;
      position: relative;
      width: 16px;
      color: #606266;
      margin: 8px 0;
      i {
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
      }
    }
  }
}
</style>

<script>
import ncformCommon from "@ncform/ncform-common";
import ace from "brace";
import "brace/mode/json";
import "brace/ext/language_tools";
import _debounce from "lodash-es/debounce";
import _isPlainObject from "lodash-es/isPlainObject";

export default {
  mixins: [ncformCommon.mixins.vue.controlMixin],

  i18nData: {
    // i18n
    en: {},
    zh_cn: {}
  },

  mounted() {
    this.editor = ace.edit(this.$refs.ace);
    this.editor.getSession().setMode("ace/mode/json");
    this.editor.setOptions({
      enableBasicAutocompletion: true,
      enableLiveAutocompletion: true
    });
    this.editor.completers = [this._getWordCompleter()];
    this.editor.setValue(
      _isPlainObject(this.value)
        ? JSON.stringify(this.value, null, 2)
        : JSON.stringify(
            {
              type: "object",
              properties: {
                name: {
                  type: "string"
                }
              }
            },
            null,
            2
          )
    );
    this.editor.getSession().on(
      "changeAnnotation",
      _debounce(() => {
        const annotations = this.editor.getSession().getAnnotations();
        if (annotations.length === 0) {
          // Correct grammar
          this.$data.schemaChanged =
            JSON.stringify(this.formSchema) !==
            JSON.stringify(JSON.parse(this.editor.getValue()));
          if (this.$data.schemaChanged) {
            this.preview();
          }
        }
      }, 500)
    );

    this.preview();
  },

  data() {
    return {
      formName: "ncform-ace_" + ncformCommon.ncformUtils.genRandomId(),
      formSchema: null,

      schemaChanged: false,

      defaultConfig: {
        // your config's default value ( Note: use mergeConfig to get config value )
        height: "300px"
      }
    };
  },

  methods: {
    preview() {
      const value = this.editor.getValue();
      this.$data.formSchema = JSON.parse(value);
      this.$data.modelVal = this.$data.formSchema;
      this.$data.schemaChanged = false;
    },

    // you can handle the modelVal before $emit it ( before this.$emit('input') )
    _processModelVal(modelVal) {
      return modelVal;
    },

    _getWordCompleter() {
      return {
        getCompletions: function(editor, session, pos, prefix, callback) {
          let wordList = [
            {
              label: "type",
              value: '"type": ""'
            },
            {
              label: "properties",
              value: '"properties": {}'
            },
            {
              label: "value",
              value: '"value": ""'
            },
            {
              label: "default",
              value: '"default": ""'
            },
            {
              label: "valueTemplate",
              value: '"valueTemplate": ""'
            },
            {
              label: "ui",
              value: '"ui": {}'
            },
            {
              label: "columns",
              value: '"columns": 12'
            },
            {
              label: "label",
              value: '"label": ""'
            },
            {
              label: "showLabel",
              value: '"showLabel": true'
            },
            {
              label: "noLabelSpace",
              value: '"noLabelSpace": true'
            },
            {
              label: "showlegendLabel",
              value: '"legend": ""'
            },
            {
              label: "showLegend",
              value: '"showLegend": true'
            },
            {
              label: "description",
              value: '"description": ""'
            },
            {
              label: "placeholder",
              value: '"placeholder": ""'
            },
            {
              label: "disabled",
              value: '"disabled": true'
            },
            {
              label: "readonly",
              value: '"readonly": true'
            },
            {
              label: "hidden",
              value: '"hidden": true'
            },
            {
              label: "help",
              value:
                '"help": ' +
                JSON.stringify(
                  {
                    show: true,
                    content: "",
                    iconCls: "",
                    text: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "itemClass",
              value: '"itemClass": ""'
            },
            {
              label: "preview",
              value:
                '"preview": ' +
                JSON.stringify(
                  {
                    type: "image",
                    value: "dx: {{$self}}",
                    clearable: true,
                    outward: {
                      width: 100,
                      height: 100,
                      shape: ""
                    }
                  },
                  null,
                  2
                )
            },
            {
              label: "video"
            },
            {
              label: "audio"
            },
            {
              label: "image"
            },
            {
              label: "link"
            },
            {
              label: "rounded"
            },
            {
              label: "circle"
            },
            {
              label: "linkFields",
              value:
                '"linkFields": ' +
                JSON.stringify(
                  [
                    {
                      fieldPath: "",
                      rules: []
                    }
                  ],
                  null,
                  2
                )
            },
            {
              label: "widget",
              value: '"widget": ""'
            },
            {
              label: "widgetConfig",
              value: '"widgetConfig": {}'
            },
            {
              label: "rules",
              value: '"rules": {}'
            },
            {
              label: "required",
              value:
                '"required": ' +
                JSON.stringify(
                  {
                    value: true,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "number",
              value:
                '"number": ' +
                JSON.stringify(
                  {
                    value: true,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "ajax",
              value:
                '"ajax": ' +
                JSON.stringify(
                  {
                    value: {
                      remoteUrl: "",
                      method: "get",
                      paramName: "",
                      otherParams: {}
                    },
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "minimum",
              value:
                '"minimum": ' +
                JSON.stringify(
                  {
                    value: 0,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "maximum",
              value:
                '"maximum": ' +
                JSON.stringify(
                  {
                    value: 100,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "multipleOf",
              value:
                '"multipleOf": ' +
                JSON.stringify(
                  {
                    value: 0,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "exclusiveMaximum",
              value:
                '"exclusiveMaximum": ' +
                JSON.stringify(
                  {
                    value: 0,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "exclusiveMinimum",
              value:
                '"exclusiveMinimum": ' +
                JSON.stringify(
                  {
                    value: 0,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "url",
              value:
                '"url": ' +
                JSON.stringify(
                  {
                    value: true,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "tel",
              value:
                '"tel": ' +
                JSON.stringify(
                  {
                    value: true,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "ipv4",
              value:
                '"ipv4": ' +
                JSON.stringify(
                  {
                    value: true,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "ipv6",
              value:
                '"ipv6": ' +
                JSON.stringify(
                  {
                    value: true,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "email",
              value:
                '"email": ' +
                JSON.stringify(
                  {
                    value: true,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "pattern",
              value:
                '"pattern": ' +
                JSON.stringify(
                  {
                    value: "",
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "hostname",
              value:
                '"hostname": ' +
                JSON.stringify(
                  {
                    value: true,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "dateTime",
              value:
                '"dateTime": ' +
                JSON.stringify(
                  {
                    value: true,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "maxLength",
              value:
                '"maxLength": ' +
                JSON.stringify(
                  {
                    value: 1,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "minLength",
              value:
                '"minLength": ' +
                JSON.stringify(
                  {
                    value: true,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "contains",
              value:
                '"contains": ' +
                JSON.stringify(
                  {
                    value: "",
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "maxItems",
              value:
                '"maxItems": ' +
                JSON.stringify(
                  {
                    value: 10,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "minItems",
              value:
                '"minItems": ' +
                JSON.stringify(
                  {
                    value: 1,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "uniqueItems",
              value:
                '"uniqueItems": ' +
                JSON.stringify(
                  {
                    value: true,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "maxProperties",
              value:
                '"maxProperties": ' +
                JSON.stringify(
                  {
                    value: 10,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "minProperties",
              value:
                '"minProperties": ' +
                JSON.stringify(
                  {
                    value: true,
                    errMsg: ""
                  },
                  null,
                  2
                )
            },
            {
              label: "customRule",
              value:
                '"customRule": ' +
                JSON.stringify(
                  {
                    script: "",
                    errMsg: "",
                    linkItems: [
                      {
                        fieldPath: "",
                        customRuleIdx: 0
                      }
                    ]
                  },
                  null,
                  2
                )
            },
            {
              label: "string"
            },
            {
              label: "number"
            },
            {
              label: "integer"
            },
            {
              label: "boolean"
            },
            {
              label: "object"
            },
            {
              label: "array"
            },
            {
              label: "HTML"
            },
            {
              label: "COMP"
            }
          ];
          callback(
            null,
            wordList.map(function(item) {
              return {
                caption: item.label,
                value: item.value || item.label,
                meta: "ncform"
              };
            })
          );
        }
      };
    }

    // you can use this.$http to make some http requests ( this.$http is the same as axios )
  }
};
</script>
