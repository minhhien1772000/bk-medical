<template>
  <div>
  <span id="background"></span>
    <v-app id="background">
      <v-row>
        <v-col md="auto">
          <v-navigation-drawer
            v-model="drawer"
            :permanent="permanent"
            :src="bg"
            absolute
            dark
          >
            <v-list dense nav class="py-0">
              <v-list-item two-line>
                <v-list-item-avatar>
                  <!--<img src="../assets/signup_img.jpg" />-->
                </v-list-item-avatar>

                <v-list-item-content>
                  <v-list-item-title>Chào bác sĩ</v-list-item-title>
                  <v-list-item-subtitle>{{timestamp}}</v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>

              <v-divider></v-divider>

              <!--<v-list-item link>-->

              <v-list-item v-for="item in items" :key="item.title" link>
                <v-list-item-icon>
                  <v-icon>{{ item.icon }}</v-icon>
                </v-list-item-icon>

                <router-link :to="item.link" style="text-decoration: none;">
                  <v-list-item-content>
                    <v-list-item-title 
                      style="color: white"
                    >
                      {{ item.title }}
                    </v-list-item-title>
                  </v-list-item-content>
                </router-link>
              </v-list-item>
            </v-list>
          </v-navigation-drawer>
        </v-col>
      </v-row>
      <v-content>
        <v-row align="center" justify="center">
          <v-col md="auto">
            <v-card style="margin-left: 120px; margin-right: -120px;" light>
              <v-card-title>
                Danh sách bệnh nhân nội trú
                <v-spacer></v-spacer>
                <v-text-field
                  v-model="search"
                  append-icon="mdi-magnify"
                  label="Search"
                  single-line
                  hide-details
                ></v-text-field>
              </v-card-title>
              <v-data-table
                :headers="headers"
                :items="users"
                :search="search"
                :single-expand="singleExpand"
                :expanded.sync="expanded"
                item-key="name"

              >
              <!--
                <template v-slot:expanded-item="{ headers, item}">
                  <td :colspan="headers.length">
                      <v-btn
                        color="primary" 
                        dark
                        style="margin-left: 50px; font-size: 12px;"
                        @click.stop="sendNoti = true"
                      >
                        Send Notification
                      </v-btn>
                      <v-btn
                        color="error"
                        dark
                        style="margin-left: 5px; font-size: 12px;"
                        @click.stop="update = true"
                      >
                        Update information
                      </v-btn>
                      <v-dialog v-model="update" width="600">
                        <v-card>
                          <v-card-title 
                            class="headline"
                          >
                            Update student information
                          </v-card-title>
                          <v-text-field
                            outlined
                            style="margin-left: 10px; margin-right: 10px; margin-bottom: -20px;"
                            label="Citizen Id"
                            v-model="citizenId"
                          >
                          
                          </v-text-field>
                          <v-text-field
                            outlined
                            style="margin-left: 10px; margin-right: 10px; margin-bottom: -20px;"
                            label="Room"
                            v-model="room"
                          >
                          
                          </v-text-field>
                          <v-text-field
                            outlined
                            style="margin-left: 10px; margin-right: 10px; margin-bottom: -20px;"
                            label="Email"
                            v-model="email"
                          >
                          
                          </v-text-field>
                          <v-text-field
                            outlined
                            style="margin-left: 10px; margin-right: 10px; margin-bottom: -20px;"
                            label="Student ID"
                            v-model="studentId"
                          >
                          
                          </v-text-field>
                          <v-text-field
                            outlined
                            style="margin-left: 10px; margin-right: 10px; margin-bottom: -20px;"
                            label="University"
                            v-model="university"
                          >
                          
                          </v-text-field>
                        <v-card-actions>
                          <v-btn text color="green" @click="updateInfo">
                            Update Information
                          </v-btn>
                        </v-card-actions>
                        </v-card>
                        </v-dialog>
                      <v-dialog v-model="sendNoti" width="600">
                        <v-card>
                          <v-card-title 
                            class="headline"
                          >
                            Your Notification
                          </v-card-title>
                          <v-text-field
                            outlined
                            style="margin-left: 10px; margin-right: 10px; margin-bottom: -20px;"
                            label="Citizen Id"
                            v-model="citizenId"
                          >
                          </v-text-field>
                          <v-text-field
                            outlined
                            style="margin-left: 10px; margin-right: 10px; margin-bottom: -20px;"
                            label="Title"
                            v-model="name"
                          >    
                          </v-text-field>
                          <v-textarea
                            outlined
                            style="margin-left: 10px; margin-right: 10px;"
                            label="Detail"
                            :value="`${informText}`"
                            v-model="detail"
                          >

                          </v-textarea>
                          <v-checkbox
                            style="margin-left: 10px; margin-top: -20px;"
                            :label="`Inform ${item.name} to his/her late payment`"
                            @click="informText = `*** YOU'RE LATE FOR PAYMENT ***`"
                          >
                          
                          </v-checkbox>
                          <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="green darken-1" text @click="sendNotification">
                              Send
                            </v-btn>
                          </v-card-actions>
                        </v-card>
                      </v-dialog>
                  </td>
                </template>
                -->
              </v-data-table>
            </v-card>
          </v-col>
        </v-row>
      </v-content>
    </v-app>
  </div>
</template>

<script>
const axios = require('axios')
export default {
  data() {
    return {
      timestamp: '',
      id: '',
      name: '',
      room: '',
      bed: '',
      in_doctor: '',
      out_doctor: '',
      insurance_id: '',
      expanded: [],
      singleExpand: false,
      drawer: true,
      items: [
        {
          title: "Bệnh nhân nội trú",
          icon: "mdi-plus-thick",
          link: "doctor-dashboard",
        },
        {
          title: "Bệnh nhân ngoại trú",
          icon: "mdi-plus-outline",
          link: "doctor-dashboard/outpatient",
        },
        {
          title: "Chỉ định thuốc",
          icon: "mdi-medicine",
          link: "doctor-dashboard/medicine"
        },
        {
          title: "Cập nhật lịch khám",
          icon: "mdi-update",
          link: "../doctor-dashboard/update-test"
        },
        {
          title: "Kiểm tra xét nghiệm",
          icon: "mdi-test-tube",
          link: "doctor-dashboard/view-test",
        },
        { title: "Đăng xuất", icon: "mdi-logout-variant", link: "/" },
      ],
      permanent: true,
      background: false,
      search: "",
      headers: [
        {
          text: "Mã số CMND",
          align: "start",
          sortable: true,
          value: "id",
        },
        { text: "Họ và tên", value: "name" },
        { text: "Phòng bệnh", value: "room" },
        { text: "Giường bệnh", value: "bed" },
        { text: "Bác sĩ nhập viện", value: "in_doctor" },
        { text: "Bác sĩ xuất viện", value: "out_doctor" },
        { text: "Mã bảo hiểm y tế", value: "insurance_id"},
      ],
      userLength: 0,
      userList: [],
      users: [

      ],
    };
  },
  computed: {

  },
  created() {
    setInterval(this.getNow, 1000)
    this.getNow()
    this.getInPatientInformation()
  },
  methods: {
    getNow() {
      
      const today = new Date();
      const date =
        today.getDate() +
        "/" +
        (today.getMonth() + 1) +
        "/" +
        today.getFullYear();
      const time = (today.getHours()<10?'0'+today.getHours():today.getHours())+ ":" + 
                (today.getMinutes()<10?'0'+today.getMinutes():today.getMinutes())
         + ":" + (today.getSeconds()<10?'0'+today.getSeconds():today.getSeconds());
      const dateTime = date + " " + time;
      this.timestamp = dateTime;
    },
    getInPatientInformation(){
      axios.get('http://localhost:3000/inPatients/')
      .then(Response => {
        this.userList = Response.data
        this.userLength = this.userList.length
        for(let i = 0; i < this.userLength; i++) {
          // console.log(this.userList[i].issn)
          this.users.push({
            id: this.userList[i].issn,
            name: this.userList[i].Patient_name,  
            room: this.userList[i].room,
            bed: this.userList[i].position,  
            in_doctor: this.userList[i].indoctorssn,
            out_doctor: this.userList[i].outdoctorssn,
            insurance_id: this.userList[i].insurance_id,
          })
        }
      })
    }  
  },
};
</script>

<style scoped>
#background {
  /*background-image: url("../assets/bg.jpg");*/
  background: #4b6cb7;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to right, #182848, #4b6cb7);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to right, #182848, #4b6cb7); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  /*position: absolute;*/
  width: 100%;
  height: 100%;
}
</style>