<template>
  <div>
    <div class="dataview">
      <breadcrumb :items="[ { label: 'Connections' } ]" />
      <div v-if="connections && connections.length > 0">
        <el-table
          :data="connections"
          style="width: 100%">
          <el-table-column
            fixed
            prop="name"
            label="Name"
            width="200">
          </el-table-column>
          <el-table-column
            prop="url"
            label="API URL">
          </el-table-column>
          <el-table-column
            prop="fctWorkerUrl"
            label="Function worker URL">
          </el-table-column>
          <el-table-column
            fixed="right"
            label="Actions"
            width="120">
            <template slot-scope="scope">
              <el-button
                v-if="!scope.row.serverConfig"
                @click.native.prevent="deleteConnection(scope.$index)"
                type="danger" plain
                size="mini">
                Delete
              </el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
      <div class="button-bar">
        <el-button type="primary" @click="addConnectionVisible = true">Add a connection</el-button>
      </div>
    </div>

    <el-dialog title="Add a connection" :visible.sync="addConnectionVisible">
      <el-form ref="connectionForm" :model="connection" :rules="connectionRules" label-width="200px">
        <el-form-item label="Connection name" prop="name">
          <el-input v-model="connection.name"></el-input>
        </el-form-item>
        <el-form-item label="Pulsar API url" prop="url">
          <el-input v-model="connection.url"></el-input>
        </el-form-item>
        <el-form-item label="Function Worker API url" prop="fctWorkerUrl">
          <el-input v-model="connection.fctWorkerUrl"></el-input>
        </el-form-item>
        <el-form-item label="Token" prop="token">
          <el-input v-model="connection.token"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="createConnection('connectionForm')">Create</el-button>
          <el-button @click="addConnectionVisible = false">Cancel</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

  </div>
</template>

<script>
import breadcrumb from '@/components/breadcrumb'
import { mapState, mapActions } from 'vuex'

export default {
  name: 'connections',

  layout: 'dataview',

  components: {
    breadcrumb
  },

  data() {
    return {
      addConnectionVisible: false,
      withSecurity: false,
      connection: {
        name: 'My local connection',
        url: 'http://localhost:8080',
        fctWorkerUrl: '',
        token: ''
      },
      connectionRules: {
        name: [
          { required: true, message: 'Please set a connection name', trigger: 'blur' }
        ],
        url: [
          { required: true, message: 'Please set the Pulsar URL', trigger: 'blur' },
          { pattern: /https?:\/\//, message: 'URL must start with http(s)://', trigger: 'blur' }
        ],
        fctWorkerUrl: [
          { required: false, trigger: 'blur' },
          { pattern: /https?:\/\//, message: 'URL must start with http(s)://', trigger: 'blur' }
        ]
      }
    }
  },

  computed: mapState('connections', ['connections']),

  mounted() {
    this.$store.dispatch('connections/fetchConnections')
  },

  methods: {
    createConnection: function (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.connection.token = this.connection.token.trim()
          this.addConnection(this._.clone(this.connection))
          this.addConnectionVisible = false
        }
        return valid
      })
    },

    ...mapActions('connections', ['deleteConnection', 'addConnection'])
  },

  head() {
    return {
      title: 'pulsar-express - connections'
    }
  }
}
</script>

<style scoped>
  
</style>