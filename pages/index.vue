<template>
  <div class="container">
    <div class="search">
      <font-awesome-icon
        class="search--icon"
        icon="search"/>
      <input
        class="search--field"
        type="text"
        v-model="search" placeholder="Search title.." />
    </div>
    <div>
      <div
        v-for="(user, index) in filteredList"
        :class="user.selected ? 'user user__selected' : 'user'"
        :key="index">
        <div
          class="user--photo"
          v-bind:style= "[{'background': 'url(' + user.avatar.replace('300x300','134x136') + ') no-repeat'}]"/>
        <div class="user--detail">
          <p class="user--detail--name">{{user.name}}</p>
          <p class="user--detail--role">{{user.title}}</p>
          <p class="user--detail--location">{{user.address}}</p>
          <hr class="user--detail--line" /> 
          <p
            class="user--detail--button"
            v-on:click="handleSelect(user)">{{user.selected ? 'SKIP SELECTION' : 'MARK AS SIUTABLE'}}</p>
          <p class="user--detail--email">{{user.email}}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      search: '',
      users: [],
      selected: {}
    };
  },
  async asyncData({ $axios }) {
    let { data } = await $axios.get("/users.json.erb");
    return { users: data };
  },
  computed: {
    filteredList() {
      return this.users.filter(user => {
        return user.name.toLowerCase().includes(this.search.toLowerCase())
      })
    }
  },
  methods: {
    handleSelect(currentUser) {
      this.users = this.users.map(user => {
        const selected = user.selected ? false : true;
        if (currentUser.name === user.name) {
          user.selected = selected
        }
        return user
      })
    },
    isSelected(user) {
      return user.name === this.selected.name
    }
  }
}
</script>

<style lang="scss">
  .container {
    align-items: center;
    background-color: white;
    height: 643px;
    margin: 54px auto 65px;
    overflow: scroll;
    padding: 19px 14px 19px 12px;
    width: 564px;
  }

  .search {
    background: #FAFAFA;
    border-radius: 2px;
    box-shadow: 0px 0px 2px rgba(0, 0, 0, 0.12), 0px 2px 2px rgba(0, 0, 0, 0.24);
    height: 48px;
    margin-bottom: 20px;
    position: relative;
    width: 100%;

    &--icon {
      color: rgba(0, 0, 0, 0.543846);
      font-size: 18px;
      left: 15px;
      position: absolute;
      top: 15px;
    }

    &--field {
      background: transparent;
      border: none;
      color: rgba(0, 0, 0, 0.75);
      font-size: 24px;
      height: 100%;
      padding-left: 50px;
      width: 100%;
    }
  }

  .user {
    background: #FAFAFA;
    border-radius: 3px;
    box-shadow: 0px 0px 2px rgba(0, 0, 0, 0.12), 0px 2px 2px rgba(0, 0, 0, 0.24);
    box-sizing: border-box;
    display: flex;
    height: 136px;
    margin-bottom: 21px;
    width: 100%;

    &__selected {
      border: 1px solid #4765FF;
    }

    &--photo {
      background-repeat: no-repeat;
      background-size: 100%;
      height: 136px;
      margin-right: 27px;
      width: 134px;
    }

    &--detail {
      padding: 10px 0 19px;
      position: relative;
      width: 100%;

      &--name {
        color: rgba(0, 0, 0, 0.87);
        font-family: 'Roboto';
        font-size: 24px;
        font-style: normal;
        font-weight: normal;
        line-height: 32px;
      }

      &--role {
        color: rgba(0, 0, 0, 0.543846);
        font-family: 'Roboto';
        font-size: 14px;
        font-style: normal;
        font-weight: bold;
        line-height: 20px;
      }

      &--location {
        color: rgba(0, 0, 0, 0.543846);
        font-family: 'Roboto';
        font-size: 14px;
        font-style: normal;
        font-weight: normal;
        line-height: 20px;
        margin-bottom: 1.5px;
      }

      &--button {
        color: #009688;
        cursor: pointer;
        font-family: 'Roboto';
        font-size: 14px;
        font-style: normal;
        font-weight: 600;
        line-height: 16px;
        margin-top: 19px;
        text-align: center;
        width: 138.62px;
      }

      &--line {
        background-color: rgba(0, 0, 0, 0.12);
      }

      &--email {
        color: rgba(0, 0, 0, 0.54);
        font-family: Roboto;
        font-size: 14px;
        font-style: normal;
        font-weight: normal;
        line-height: 16px;
        position: absolute;
        right: 9px;
        top: 10px;
      }
    }
  }
</style>
