<script>
export default {
    props: {
        filterStopName: {
            type: Array,
            default: []
        },
        toggleTotalSort: {
            type: Boolean,
            default: false
        },
        toggleCurrentSort: {
            type: Boolean,
            default: false
        }
    },
    methods: {
        timeFormat(t) {
            var date = [],
                time = [];

            date.push(t.substr(0, 4));
            date.push(t.substr(4, 2));
            date.push(t.substr(6, 2));
            time.push(t.substr(8, 2));
            time.push(t.substr(10, 2));
            time.push(t.substr(12, 2));

            return date.join("/") + " " + time.join(":");
        },
    }
};
</script>

<template>
    <table class="table table-striped">
        <thead>
            <tr>
                <th>#</th>
                <th>場站名稱</th>
                <th>場站區域</th>
                <th>
                    目前可用車輛
                    <a
                        href="javascript:;"
                        class="btn-controller"
                        @click="$emit('sortCurrent')"
                    >
                        <i
                            class="fas fa-caret-up"
                            v-show="toggleCurrentSort"
                        ></i>
                        <i
                            class="fas fa-caret-down"
                            v-show="!toggleCurrentSort"
                        ></i>
                    </a>
                </th>
                <th>
                    總停車格
                    <a
                        href="javascript:;"
                        class="btn-controller"
                        @click="$emit('sortTotal')"
                    >
                        <i class="fas fa-caret-up" v-show="toggleTotalSort"></i>
                        <i
                            class="fas fa-caret-down"
                            v-show="!toggleTotalSort"
                        ></i>
                    </a>
                </th>
                <th>資料更新時間</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="s in filterStopName">
                <td>{{ s.sno }}</td>
                <td>{{ s.sna }}</td>
                <td>{{ s.sarea }}</td>
                <td>{{ s.sbi }}</td>
                <td>{{ s.tot }}</td>
                <td>{{ timeFormat(s.mday) }}</td>
            </tr>
        </tbody>
    </table>
</template>

<style></style>
