<template>
  <TabPage :active-menu.sync="config.activeMenu" :submenu="config.submenu">
    <el-badge v-if="props.tab === 'AssignedTicketList'" slot="badge" slot-scope="props" :value="getBadgeValue(props)" size="mini" type="primary" />
    <keep-alive>
      <component :is="config.activeMenu" />
    </keep-alive>
  </TabPage>
</template>

<script>
import { TabPage } from '@/layout/components'
import AssignedTicketList from './AssignedTicketList'
import MyTicketList from './MyTicketList'
import { getTicketOpenCount } from '@/api/ticket'

export default {
  name: 'Index',
  components: {
    TabPage,
    AssignedTicketList,
    MyTicketList
  },
  data() {
    return {
      assignedTicketCount: 0,
      config: {
        activeMenu: 'MyTicketList',
        submenu: [
          {
            title: this.$t('tickets.MyTickets'),
            name: 'MyTicketList'
          },
          {
            title: this.$t('tickets.AssignedMe'),
            name: 'AssignedTicketList'
          }
        ]
      }
    }
  },
  mounted() {
    this.getTicketOpenCount()
  },
  methods: {
    getTicketOpenCount() {
      getTicketOpenCount(1).then(data => {
        this.assignedTicketCount = data.count
      })
    },
    getBadgeValue(props) {
      return this.assignedTicketCount
    }
  }
}
</script>

<style scoped>
  .primary {
    background-color: #1ab394;
  }

</style>
