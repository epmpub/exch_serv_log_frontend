<template>
  <el-form
    :model="ruleForm"
    :rules="rules"
    ref="ruleForm"
    label-width="100px"
    class="demo-ruleForm"
  >
    <el-form-item label="Sender" prop="sender">
      <el-input
        v-model.lazy="ruleForm.sender"
        @change="sender_change"
      ></el-input>
    </el-form-item>

    <el-form-item label="Recipients" prop="recipients">
      <el-input
        v-model.lazy="ruleForm.recipients"
        @change="recipients_change"
      ></el-input>
    </el-form-item>

    <el-form-item label="Server" prop="server">
      <el-input v-model="ruleForm.server" :disabled="true"></el-input>
    </el-form-item>

    <el-form-item label="Star(默认时间无法查询)" required>
      <el-col :span="11">
        <el-form-item prop="date1">
          <el-date-picker
            type="datetime"
            placeholder="选择日期"
            v-model="ruleForm.date1"
            value-format="yyyy/MM/dd HH:mm:ss"
            style="width: 100%"
          ></el-date-picker>
        </el-form-item>
      </el-col>
    </el-form-item>

    <el-form-item label="End(默认时间无法查询)" required>
      <el-col :span="11">
        <el-form-item prop="date2">
          <el-date-picker
            type="datetime"
            placeholder="选择日期"
            v-model="ruleForm.date2"
            value-format="yyyy/MM/dd HH:mm:ss"
            style="width: 100%"
          ></el-date-picker>
        </el-form-item>
      </el-col>

    </el-form-item>

    <el-form-item label="EventId" prop="eventId">
      <el-select v-model="ruleForm.eventId" placeholder="请选择EventID类型">
        <el-option label="NOTIFYMAPI" value="notifymapi"></el-option>
        <el-option label="SEND" value="send"></el-option>
        <el-option label="RECEIVE" value="receive"></el-option>
        <el-option label="SUBMIT" value="submit"></el-option>
        <el-option label="HAREDIRECTFAIL" value="haredirectfail"></el-option>
        <el-option label="DELIVER" value="deliver"></el-option>
      </el-select>
    </el-form-item>

    <el-form-item>
      <el-button type="primary" @click="submitForm('ruleForm')">查询</el-button>
      <el-button @click="resetForm('ruleForm')">重置</el-button>
    </el-form-item>

    <el-table :data="tableData" style="width: 100%">
      <el-table-column prop="sender" label="Sender" width="180">
      </el-table-column>

      <el-table-column prop="recipients" label="Recipients" width="180">
      </el-table-column>

      <el-table-column prop="timestamp" label="Timestamp" width="180">
      </el-table-column>

      <el-table-column prop="messageSubject" label="MessageSubject"> </el-table-column>
    </el-table>
  </el-form>
</template>

<script>
export default {
  name: "MyForm2",
  data() {
    return {
      tableData: [
        {
          sender: "acc@z.cn",
          recipients: "abc@z.cn",
          timestamp: "2022/05/17 16:17:00",
          messageSubject: "this is a test Mail.",
        },
      ],

      ruleForm: {
        server: "",
        sender: "",
        recipients: "",
        date1: new Date(),
        dateTime1: "",

        date2: new Date(),
        dateTime2: "",

        eventId: "",
      },
      rules: {
        sender: [
          { required: true, message: "请输入发送者邮件地址", trigger: "blur" },
          {
            type: "email",
            message: "请输入正确的邮箱地址",
            trigger: ["blur", "change"],
          },
        ],
        recipients: [
          { required: true, message: "请输入收件人邮件地址", trigger: "blur" },
          {
            type: "email",
            message: "请输入正确的邮箱地址",
            trigger: ["blur", "change"],
          },
        ],
        eventId: [
          { required: true, message: "请选择事件ID", trigger: "change" },
        ],
        date1: [
          {
            // type: "date",
            required: true,
            message: "请选择日期",
            trigger: "change",
          },
        ],
        dateTime1: [
          {
            type: "date",
            required: true,
            message: "请选择时间",
            trigger: "change",
          },
        ],
        date2: [
          {
            // type: "date",
            required: true,
            message: "请选择日期",
            trigger: "change",
          },
        ],
        dateTime2: [
          {
            type: "date",
            required: true,
            message: "请选择时间",
            trigger: "change",
          },
        ],
      },
    };
  },
  methods: {
    sender_change() {
      console.log("sender被修改了", this.ruleForm.sender);
      const data = {
        sender: this.ruleForm.sender
      };
      fetch("http://localhost:5000/api/ExchServer", {
        method: "POST", // or 'PUT'
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(data),
      })
        .then((response) => response.json())
        .then((data) => {
          this.ruleForm.server = (data["output"][0]);
          console.log("Success:", data);
        })
        .catch((error) => {
          console.error("Error:", error);
        });
    },
    recipients_change() {
      console.log("recipients被修改了", this.ruleForm.recipients);
            const data = {
        sender: this.ruleForm.recipients
      };
      fetch("http://localhost:5000/api/ExchServer", {
        method: "POST", // or 'PUT'
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(data),
      })
        .then((response) => response.json())
        .then((data) => {
          this.ruleForm.server = (data["output"][0]);
          console.log("Success:", data);
        })
        .catch((error) => {
          console.error("Error:", error);
        });

    },

    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          // this.ruleForm.server = "exch01";
          // this.tableData.push({
          //   start: "2020/05/17/ 16:17:59",
          //   sender: "tony@z.cn",
          //   recipients: "bill@g.cn",
          //   subject: "windows",
          // });
          // console.log(this.ruleForm.eventId);

          const data = {
            sender: this.ruleForm.sender,
            recipients: this.ruleForm.recipients,
            server:this.ruleForm.server,
            eventId: this.ruleForm.eventId,
            start: this.ruleForm.date1,
            end: this.ruleForm.date2
          };
          console.log(data)

          fetch("http://localhost:5000/api/query", {
            method: "POST", // or 'PUT'
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify(data),
          })
            .then((response) => response.json())
            .then((data) => {
              console.log(data["output"]);
              data["output"].forEach(element => {
                console.log(element)
                
              });
              this.tableData.push({
                start: "2020/05/17/ 16:17:59",
                sender: "tony@z.cn",
                recipients: "bill@g.cn",
                subject: "windows",
              });








              console.log("Success:", data);
            })
            .catch((error) => {
              console.error("Error:", error);
            });

          // alert("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
  },
  watch: {
    //API调用过于密集，容易引发性能问题；
    // 'ruleForm.sender':{
    //   handler:function(newValue){
    // 			console.log('sender被修改了',newValue)
    //   },
    //   deep:true
    // }
  },
};
</script>