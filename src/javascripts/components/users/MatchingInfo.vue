<template>
  <div>
    <div class="tabs-box">
      <div class="tabs">
        <div
          v-for="page in pages"
          v-bind:key="page.id"
          class="tab-item"
          :active="page.id === current_page"
          @click.prevent="changePage(page.id)"
        >{{ page.label }}</div>
      </div>
    </div>
    <div v-show="current_page == 2">
      <!-- TODO v-for -->
      <matching-list
          v-for="room in rooms"
          :user="room.user"
          :badge="room.count"
          :key="room.id"
          :room_id="room.id"
      />
    </div>
    <user-chat-room-list v-show="current_page == 1"></user-chat-room-list>
  </div>
</template>

<script>
import http from '../../http'
import UserChatRoomList from './ChatRoomList.vue'
import MatchingList from '../MatchingList.vue'

export default {
  data () {
    return {
      // TODO ↓ 2
      current_page: 1,
      pages: [
        {
          id: 1,
          label: 'マッチングリスト'
        },
        {
          id: 2,
          label: 'メッセージ'
        }
      ],
      users: [],
      rooms: []
    }
  },
  async mounted () {
    const response = await http.get('/api/users/chats')
    this.rooms = await response.data.rooms
    console.log(response.data)
  },
  methods: {
    async changePage (id) {
      this.current_page = id
    }
  },
  components: {
    UserChatRoomList,
    MatchingList,
  }
}
</script>

<style scoped>
@import '../../../style/users/matching_info.scss'
</style>
