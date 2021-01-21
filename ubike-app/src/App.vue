<template>
    <div id="app">
        <SearchBar :searchTxt="searchTxt" @filterTxt="updateSearchTxt" />
        <DataLists
            :filterStopName="filterStopName"
            :toggleTotalSort="toggleTotalSort"
            :toggleCurrentSort="toggleCurrentSort"
            @sortCurrent="sortBikes"
            @sortTotal="sortBikes"
        />
        <Pagination
            :pageIdx="pageIdx"
            :getTotalPageNum="getTotalPageNum"
            :getPageStartNum="getPageStartNum"
            :getPageEndNum="getPageEndNum"
            @decreseIdx="decresePageIdx"
            @increseIdx="incresePageIdx"
            @equalIdx="equalIdxToNum"
        />
    </div>
</template>

<script>
import SearchBar from "./components/SearchBar";
import DataLists from "./components/DataLists";
import Pagination from "./components/Pagination";

export default {
    name: "App",
    components: {
        SearchBar,
        DataLists,
        Pagination,
    },
    data() {
        return {
            ubikeStops: [],
            searchTxt: "",
            toggleCurrentSort: false,
            toggleTotalSort: false,
            pageIdx: 1,
            pageEachNum: 20,
            pageScopeLen: 5,
        };
    },
    methods: {
        // sortCurrentBikes() {
        //     this.toggleCurrentSort = !this.toggleCurrentSort;

        //     if (this.toggleCurrentSort) {
        //         this.ubikeStops.sort((a, b) => a.sbi - b.sbi);
        //     } else {
        //         this.ubikeStops.sort((a, b) => b.sbi - a.sbi);
        //     }
        // },
        // sortTotalBikeGarges() {
        //     this.toggleTotalSort = !this.toggleTotalSort;

        //     if (this.toggleTotalSort) {
        //         this.ubikeStops.sort((a, b) => a.tot - b.tot);
        //     } else {
        //         this.ubikeStops.sort((a, b) => b.tot - a.tot);
        //     }
        // },
        sortBikes(arr) {
            let sortWay = false;
            const sortType = arr[0];
            const sortName = arr[1];

            console.log(sortType, sortName);

            switch(sortType) {
                case 'current':
                    this.toggleCurrentSort = !this.toggleCurrentSort;
                    sortWay = this.toggleCurrentSort;
                    break;
                case 'total':
                    this.toggleTotalSort = !this.toggleTotalSort;
                    sortWay = this.toggleTotalSort;
                    break;
            }

            if (sortWay) {
                this.ubikeStops.sort((a, b) => a[sortName] - b[sortName]);
            } else {
                this.ubikeStops.sort((a, b) => b[sortName] - a[sortName]);
            }
        },
        incresePageIdx() {
            if (this.pageIdx >= this.getTotalPageNum) {
                return;
            }

            this.pageIdx++;
        },
        decresePageIdx() {
            if (this.pageIdx <= 1) {
                return;
            }

            this.pageIdx--;
        },
        equalIdxToNum(val) {
          this.pageIdx = val;
        },
        updateSearchTxt(val) {
            this.searchTxt = val;
        }
    },
    computed: {
        filterStopName() {
            if(!this.ubikeStops.length) {
                return;
            }

            // console.log(this.searchTxt,this.ubikeStops);

            const filterUbikeStopsArr = this.ubikeStops.filter((item) =>
                item.sna.includes(this.searchTxt)
            );
            const startIdx = (this.pageIdx - 1) * this.pageEachNum;
            const endIdx = startIdx + this.pageEachNum;
            let pageEachStopsArr = [];

            for (let i = startIdx; i < endIdx; i++) {
                pageEachStopsArr.push(filterUbikeStopsArr[i]);
            }

            return pageEachStopsArr;
        },
        getTotalPageNum() {
            return Math.ceil(this.ubikeStops.length / this.pageEachNum);
        },
        getPageStartNum() {
            let startNum = this.pageIdx;
            const num = this.pageScopeLen - 1;

            if (startNum + num >= this.getTotalPageNum) {
                startNum = this.getTotalPageNum - num;
            }

            return startNum;
        },
        getPageEndNum() {
            const num = this.pageScopeLen - 1;
            let endNum = this.pageIdx + num;

            if (endNum >= this.getTotalPageNum) {
                endNum = this.getTotalPageNum;
            }

            return endNum;
        },
    },
    created() {
        // 欄位說明請參照:
        // http://data.taipei/opendata/datalist/datasetMeta?oid=8ef1626a-892a-4218-8344-f7ac46e1aa48

        // sno：站點代號、 sna：場站名稱(中文)、 tot：場站總停車格、
        // sbi：場站目前車輛數量、 sarea：場站區域(中文)、 mday：資料更新時間、
        // lat：緯度、 lng：經度、 ar：地(中文)、 sareaen：場站區域(英文)、
        // snaen：場站名稱(英文)、 aren：地址(英文)、 bemp：空位數量、 act：全站禁用狀態

        fetch("https://tcgbusfs.blob.core.windows.net/blobyoubike/YouBikeTP.gz")
            .then((res) => res.json())
            .then((res) => {
                // 將 json 轉陣列後存入 this.ubikeStops
                this.ubikeStops = Object.keys(res.retVal).map(
                    (key) => res.retVal[key]
                );
            });
    },
};
</script>

<style>
@import url("https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.1/css/all.min.css");
@import url("https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta/css/bootstrap.min.css");
</style>
