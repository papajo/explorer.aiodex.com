<template>
    <!-- Page -->


    <div class="panel panel-bordered">
        <header class="panel-heading">

            <h3 class="panel-title">Masternodes</h3>
        </header>
        <div class="table-responsive">
            <mix-table :data="data" css="table table-hover table-striped table-bordered" ref="mixtable" @mixtable:fetch="fetch"
                :limit=15>
                <mix-table-column data-field="idx" label="Win at" width="110px"></mix-table-column>
                <mix-table-column data-field="wid" label="Addr" type="slot" target="addr"></mix-table-column>
                <mix-table-column data-field="stt" label="Status" type="slot" width="15%" target="stt"></mix-table-column>
                <mix-table-column data-field="at" label="Active" type="slot" width="15%" target="at"></mix-table-column>
                <!-- <mix-table-column data-field="ls" label="Last Seen" type="slot" width="15%" target="ls"></mix-table-column> -->
                <mix-table-column data-field="lp" label="Reward" type="slot" width="15%" target="lp"></mix-table-column>


                <template slot="stt" slot-scope="props">
                    <span v-if="props.value == 'ENABLED' || props.value == 'WATCHDOG_EXPIRED'" class="text-success">{{props.value}}</span>
                    <span v-else class="text-warning">{{props.value}}</span>
                </template>

                <template slot="at" slot-scope="props">
                    <span>Last: {{props.row.ls | moment('from','now')}}</span><br>
                    <span>Active: {{props.value *1000 | duration('humanize') }}</span>

                </template>
                <template slot="ls" slot-scope="props">
                    <span>{{props.value | moment('from','now')}}</span>
                </template>
                <template slot="lp" slot-scope="props">
                    <span>Last: {{props.value | moment('from','now')}}</span><br>
                    <span>Block: {{props.row.lb}}</span>
                </template>
                <template slot="addr" slot-scope="props">
                    IP: {{props.row.ip}}<br>
                    Payee: <router-link :to="{ name: 'addr-info', params: { id: props.value }}">{{props.value}}</router-link>
                </template>
            </mix-table>

        </div>
    </div>

    <!-- End Page -->

</template>


<script>

    import { mapGetters } from "vuex";

    export default {

        data: function () {
            return {
                timer: null,
            }
        },
        computed: {
            ...mapGetters({
                data: "mn_data",
            })
        },
        mounted() {

            var _this = this;
            clearInterval(_this.timer);
            _this.timer = setInterval(function () {
                if (_this.$refs.mixtable)
                    _this.$refs.mixtable.reload();
            }, 60000);
        },

        methods: {
            fetch(params) {
                this.$store.dispatch("MN_FETCH", Object.assign(params, { cid: this.$route.params.cid }));
            }
        },

    };
</script>