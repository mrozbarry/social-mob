<template>
    <div class="flex justify-center flex-wrap">
        <div class="day text-center mx-1 mb-2 px-2"
             v-for="date in weekDates"
             :class="moment(date).weekday() % 2 ? 'bg-blue-100' : 'bg-blue-200'">
            <h3 class="text-lg font-semibold mt-6 mb-3" v-text="moment(date).format('dddd')"></h3>
            <div v-if="user">
                <button class="create-mob bg-green-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mb-3"
                        @click="$modal.show(`create-mob-${date}`)">
                    Create new mob
                </button>
                <modal :dynamic="true" :width="450" :height="350" :name="`create-mob-${date}`">
                    <div class="flex w-full h-full items-center">
                        <create-mob class="create-mob"
                                    @mob-created="onMobCreated"
                                    :owner="user"
                                    :start-date="date"/>
                    </div>
                </modal>
            </div>

            <mob-card v-for="socialMob in socialMobs[date]"
                      :key="socialMob.id"
                      :socialMob="socialMob"
                      class="my-3"/>
        </div>
    </div>
</template>

<script lang="ts">
    import {Component, Prop, Vue} from 'vue-property-decorator';
    import {ISocialMob, IUser, IWeekMobs} from '../types';
    import axios from 'axios';
    import MobCard from './MobCard.vue';
    import CreateMob from './CreateMob.vue';
    import moment from 'moment';

    @Component({
        components: {CreateMob, MobCard}
    })
    export default class WeekView extends Vue {
        @Prop({required: false, default: null}) user!: IUser;

        socialMobs: IWeekMobs = {};
        moment = moment;

        created() {
            this.getAllMobsOfTheWeek();
        }

        async getAllMobsOfTheWeek() {
            const response = await axios.get<IWeekMobs>('/social_mob/week');
            this.socialMobs = response.data;
        }

        async onMobCreated(mob: ISocialMob) {
            await this.getAllMobsOfTheWeek();
            let date = moment(mob.start_time).format('YYYY-MM-DD');
            this.$modal.hide(`create-mob-${date}`);
        }

        get weekDates(): string[] {
            return Object.keys(this.socialMobs);
        }
    }
</script>

<style scoped>
    .day {
        width: 20rem;
        min-height: 35rem;
    }
</style>