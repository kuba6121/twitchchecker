<template>
    <main>
        <SearchUser :loading="loading" v-on:fetchFollows="fetchData" />
        <Follows v-if="follows != null && follows.length > 0 && error === null" :follows="follows" />
        <h2 v-if="follows != null && !loading && follows.length === 0 && error === null" class="info">No follows</h2>
        <h2 v-if="error != null" class="error">An error occurred. Try again later!</h2>
    </main>
</template>

<script>
import SearchUser from '@/components/SearchUser';
import Follows from '@/components/Follows';
import axios from 'axios';
export default {
    name: 'Main',
    components: {
        SearchUser,
        Follows
    },
    data() {
        return {
            follows: null,
            loading: false,
            page: 0,
            error: null
        }
    },
    methods: {
        async fetchData(user) {
            try {
                this.follows = [];
                this.loading = true;
                this.page = 0;
                const res = await axios.get(`https://api.twitch.tv/kraken/users?login=${user}`);
                const id = res.data.users[0]['_id'];
                const channels = await axios.get(`https://api.twitch.tv/kraken/users/${id}/follows/channels?limit=100&offset=${this.page * 100}&sortby=last_broadcast`);
                this.page++;
                let follows = channels.data.follows;
                const pages = Math.ceil(channels.data['_total'] / 100);
                do {
                    await axios.get(`https://api.twitch.tv/kraken/users/${id}/follows/channels?limit=100&offset=${this.page * 100}&sortby=last_broadcast`).then(response => {
                        follows = follows.concat(response.data.follows);
                    });
                    this.page++;
                } while(this.page < pages);
                this.follows = follows;
                this.loading = false;
            } catch(err) {
                this.loading = false;
                this.error = err;
            }
        }
    }
}
</script>

<style lang="scss">
main {
    min-height: calc(100vh - 50px);
    position: relative;
    .info {
        text-align: center;
        color: #fffffe;
        padding-top: 220px;
        margin: 0;
        font-size: 26px;
    }
    .error {
        text-align: center;
        color: #f25042;
        padding-top: 220px;
        margin: 0;
        font-size: 26px;
    }
}
</style>