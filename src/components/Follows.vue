<template>
    <div class="follows">
        <div class="follows__total">
            <h2>Total follows: {{follows.length}}</h2>
        </div>
        <div class="sort">
            <p>Sort by: </p>
            <div class="sort__select">
                <CustomSelect :options="options" v-model="sortBy" />
            </div>
        </div>
        <div class="channels">
            <div v-for="follow in sortedFollows" :key="follow.channel['_id']" class="channel">
                <a class="channel__img" target="_blank" :href='"https://www.twitch.tv/" + follow.channel.name'>
                    <img :src="follow.channel.logo">
                </a>
                <a :href='"https://www.twitch.tv/" + follow.channel.name' target="_blank" class="name">{{follow.channel.display_name}}</a>
                <p class="days">{{followDays(follow.created_at)}} days</p>
            </div>
        </div>
    </div>
</template>

<script>
import CustomSelect from '@/components/Select';
export default {
    name: 'Follows',
    components: {
        CustomSelect
    },
    props: {
        follows: Array
    },
    data() {
        return {
            sortBy: 'days_desc',
            options: [
                {name: 'Days ascending', value: 'days_asc'},
                {name: 'Days descending', value: 'days_desc'},
                {name: 'A-Z', value: 'name_asc'},
                {name: 'Z-A', value: 'name_desc'}
            ]
        }
    },
    methods: {
        followDays(time) {
            const currentDate = new Date();
            const followDate = new Date(time.substring(0, 10));
            let diff = new Date(currentDate - Date.parse(followDate)) / (1000 * 3600 * 24);
            diff = Math.trunc(diff);
            return diff;
        }
    },
    computed: {
        sortedFollows() {
            const sorted = this.follows;
            if(this.sortBy === 'days_asc') {
                return sorted.sort((a, b) => this.followDays(a.created_at) - this.followDays(b.created_at));
            }
            if(this.sortBy === 'days_desc') {
                return sorted.sort((a, b) => this.followDays(a.created_at) - this.followDays(b.created_at)).reverse();
            }
            if(this.sortBy === 'name_asc') {
                return sorted.sort((a, b) => a.channel.display_name.localeCompare(b.channel.display_name));
            }
            if(this.sortBy === 'name_desc') {
                return sorted.sort((a, b) => b.channel.display_name.localeCompare(a.channel.display_name));
            }
            return sorted;
        }
    }
}
</script>

<style lang="scss" scoped>
.follows {
    color: #fffffe;
    padding-top: 200px;
    .follows__total {
        display: flex;
        justify-content: center;
        h2 {
            padding: 5px 20px;
            background-color: #09090B;
            border-radius: 35px;
            margin-bottom: 0;
        }
    }
    .sort {
        display: flex;
        justify-content: center;
        align-items: center;
        margin: 16px 0 16px 0;
        p {
            margin: 0;
            margin-right: 10px;
        }
        .sort__select {
            width: 200px;
        }
    }
    .channels {
        padding-left: 10px;
        padding-right: 10px;
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        .channel {
            background-color: #09090B;
            height: 70px;
            border-radius: 35px;
            flex-basis: calc(100% - 10px);
            color: #fff;
            display: flex;
            align-items: center;
            margin-bottom: 10px;
            margin-left: 5px;
            margin-right: 5px;
            padding: 10px;
            box-sizing: border-box;
            max-width: calc(100% - 10px);
            .channel__img {
                border-radius: 50%;
                height: 100%;
                img {
                    border-radius: 50%;
                    height: 100%;
                }
                &:hover + .name {
                    color: #fffffe;
                }
            }
            .name {
                padding-left: 10px;
                transition: all .2s ease;
                text-overflow: ellipsis;
                overflow: hidden;
                &:hover {
                    color: #fffffe;
                }
            }
            .days {
                margin-left: auto;
                background-color: #16161a;
                padding: 5px 10px;
                border-radius: 10px;
                margin-right: 5px;
                font-family: 'Poppins';
                white-space: nowrap;
            }
        }
    }
    
}
@media only screen and (min-width: 600px) {
    .follows {
        .channels {
            .channel {
                flex-basis: calc(50% - 10px);
                max-width: calc(50% - 10px);
            }
        }
        .sort {
            justify-content: flex-end;
            padding-right: 20px;
        }
    }
}
@media only screen and (min-width: 950px) {
    .follows {
        .channels {
            .channel {
                flex-basis: calc(30% - 10px);
                max-width: calc(30% - 10px);
            }
        }
    }
}
@media only screen and (min-width: 1500px) {
    .follows {
        .channels {
            .channel {
                flex-basis: calc(20% - 10px);
                max-width: calc(20% - 10px);
            }
        }
    }
}
</style>