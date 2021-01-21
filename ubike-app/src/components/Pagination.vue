<script>
export default {
    props: {
        pageIdx: {
            type: Number,
            default: 1
        },
        getTotalPageNum: {
            type: Number,
            default: 1
        },
        getPageStartNum: {
            type: Number,
            default: 1
        },
        getPageEndNum: {
            type: Number,
            default: 1
        }
    }
};
</script>

<template>
    <div class="d-flex justify-content-center mt-5">
        <nav aria-label="Page navigation example">
            <ul class="pagination">
                <li class="page-item" :class="{ disabled: pageIdx === 1 }">
                    <a
                        class="page-link"
                        href="#"
                        aria-label="Previous"
                        @click="$emit('decreseIdx')"
                    >
                        <span aria-hidden="true">&laquo;</span>
                    </a>
                </li>
                <li
                    class="page-item"
                    v-for="num in getTotalPageNum"
                    :key="num"
                    :class="{ active: pageIdx === num }"
                    v-show="num <= getPageEndNum && num >= getPageStartNum"
                >
                    <a class="page-link" href="#" @click="$emit('equalIdx', num)">{{
                        num
                    }}</a>
                </li>
                <li
                    class="page-item"
                    :class="{ disabled: pageIdx === getTotalPageNum }"
                >
                    <a
                        class="page-link"
                        href="#"
                        aria-label="Next"
                        @click="$emit('increseIdx')"
                    >
                        <span aria-hidden="true">&raquo;</span>
                    </a>
                </li>
            </ul>
        </nav>
    </div>
</template>

<style></style>
