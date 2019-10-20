<template>
    <div class="v-table" :class="{'loading' : loading}">
        <div class="v-row v-table__header" :class="headerClass" :style="setColumnsNumber()">
            <div class="v-table-heading" v-for="(header, index) in headers" :key="index"  @click="sortable ? sort(header.for) : $emit('sort', header.for)">
                <slot :name="`HEADER_${header.for}`" :item="header">
                    {{ header.label }}
                </slot>
            </div>
        </div>
        <div class="v-row" v-for="(item, index) in sortedItems" :key="index" :style="setColumnsNumber()" :class="rowClass" :id="addId(index)" @click="$emit('rowClick', item, index, $event)" @dblclick="$emit('dblRowClick', item, index, $event)">
            <div class="v-table-data" v-for="(o, index) in headers" :key="index">
                <slot :name="`${o.for}`" :item="item">
                    {{ item }}
                </slot>
            </div>
        </div>
        <div v-if="loading" class="v-loading">
            <slot name="loading">
            </slot>
        </div>
        <div v-if="items.length == 0" class="none-data">
            <slot name="no-records">
            <p>Brak rekordów</p>
                <svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" width="100%"
                height="100%" viewBox="0 0 330 512" enable-background="new 0 0 330 512" xml:space="preserve">
                <path fill="#B2B1B2" stroke="#9FA1A4" stroke-width="10" stroke-miterlimit="10" d="M174.348,286.147l25.845,28.199"/>
                <path fill="#B2B1B2" d="M220.579,170.092H109.542c-10.326,0-18.727,8.4-18.727,18.728v153.71c0,10.326,8.4,18.728,18.727,18.728
                    h111.038c10.325,0,18.727-8.401,18.727-18.728V188.819C239.306,178.493,230.904,170.092,220.579,170.092L220.579,170.092z
                    M109.542,174.724h111.038c7.771,0,14.095,6.323,14.095,14.096v16.139H95.446v-16.14
                    C95.446,181.047,101.77,174.724,109.542,174.724L109.542,174.724z M220.579,356.624H109.542c-7.772,0-14.095-6.323-14.095-14.095
                    v-132.94h139.228v132.94C234.674,350.302,228.351,356.625,220.579,356.624L220.579,356.624z"/>
                <path fill="#B2B1B2" d="M102.202,189.383c-0.024,3.391,2.705,6.16,6.096,6.185c3.391,0.024,6.16-2.705,6.185-6.096
                    c0-0.03,0-0.06,0-0.089c0.024-3.391-2.705-6.16-6.096-6.185c-3.391-0.024-6.16,2.705-6.185,6.096
                    C102.202,189.323,102.202,189.353,102.202,189.383z M121.062,189.383c-0.025,3.391,2.704,6.16,6.095,6.185
                    c3.392,0.024,6.161-2.705,6.185-6.096c0-0.03,0-0.06,0-0.089c0.025-3.391-2.704-6.16-6.096-6.185
                    c-3.391-0.024-6.16,2.705-6.185,6.096C121.061,189.323,121.061,189.353,121.062,189.383z M139.921,189.383
                    c-0.025,3.391,2.705,6.16,6.096,6.185c3.391,0.024,6.16-2.705,6.185-6.096c0-0.03,0-0.06,0-0.089
                    c0.025-3.391-2.704-6.16-6.095-6.185c-3.392-0.024-6.161,2.705-6.185,6.096C139.921,189.323,139.921,189.353,139.921,189.383z
                    M230.046,189.384c-0.001,1.695-1.374,3.069-3.069,3.069h-47.247c-1.696-0.035-3.042-1.438-3.007-3.133
                    c0.033-1.646,1.36-2.973,3.007-3.007h47.247c1.695,0,3.069,1.374,3.069,3.069C230.046,189.383,230.046,189.384,230.046,189.384
                    L230.046,189.384z M106.644,137.813l3.629-7.067l0.621,7.92l7.065,3.628l-7.917,0.623l-3.631,7.064l-0.62-7.919l-7.065-3.627
                    L106.644,137.813z M55.151,173.693l2.433-4.737L58,174.266l4.737,2.433l-5.308,0.417l-2.435,4.737l-0.415-5.31l-4.737-2.432
                    L55.151,173.693z M302.703,166.582l5.37-2.996l-2.998,5.369l2.998,5.371l-5.37-2.997l-5.371,2.997l2.998-5.37l-2.998-5.37
                    L302.703,166.582z M64.054,265.736l5.37-2.996l-2.998,5.369l2.998,5.369l-5.37-2.994l-5.369,2.994l2.998-5.369l-2.998-5.369
                    L64.054,265.736z M275.408,352.08c-4.869,0-8.83-3.961-8.83-8.83s3.961-8.83,8.83-8.83c4.868,0,8.83,3.961,8.83,8.83
                    C284.238,348.118,280.277,352.08,275.408,352.08z M275.408,337.894c-2.955,0-5.357,2.402-5.357,5.356
                    c0,2.953,2.402,5.356,5.357,5.356c2.953,0,5.355-2.403,5.355-5.356C280.764,340.296,278.36,337.894,275.408,337.894
                    L275.408,337.894z M18.162,264.512c-3.323-0.005-6.015-2.698-6.018-6.021c0.003-3.322,2.695-6.015,6.018-6.02
                    c3.323,0.003,6.016,2.697,6.02,6.02C24.182,261.81,21.48,264.512,18.162,264.512z M18.162,254.785
                    c-2.044,0.002-3.701,1.66-3.702,3.704c0,2.044,1.661,3.705,3.702,3.705c2.045-0.003,3.702-1.66,3.704-3.705
                    C21.863,256.444,20.206,254.788,18.162,254.785z M47.676,333.637c-5.708,0-10.349-4.644-10.349-10.35
                    c0-5.708,4.643-10.351,10.349-10.351s10.349,4.644,10.349,10.351C58.025,328.993,53.384,333.637,47.676,333.637z M47.676,316.409
                    c-3.796,0.005-6.872,3.081-6.875,6.877c0,3.791,3.084,6.876,6.875,6.876s6.875-3.085,6.875-6.876
                    C54.548,319.49,51.472,316.414,47.676,316.409z M26.069,156.021c0,1.634,1.324,2.958,2.958,2.959c1.634,0,2.958-1.324,2.958-2.958
                    c0,0,0,0,0-0.001c0-1.634-1.324-2.958-2.958-2.958C27.394,153.063,26.069,154.387,26.069,156.021
                    C26.069,156.021,26.069,156.021,26.069,156.021z M306.275,273.164c0,2.792,2.262,5.055,5.054,5.055s5.054-2.263,5.054-5.055
                    c0-2.791-2.262-5.054-5.054-5.054S306.275,270.373,306.275,273.164z M268.315,220.356c-4.886,0-8.858-3.974-8.858-8.858
                    c0-4.885,3.975-8.859,8.858-8.859s8.858,3.975,8.858,8.859C277.174,216.383,273.199,220.356,268.315,220.356z M268.315,204.954
                    c-3.612,0.004-6.54,2.931-6.544,6.543c0.006,3.611,2.932,6.538,6.544,6.542c3.611-0.005,6.538-2.931,6.542-6.542
                    C274.854,207.886,271.928,204.958,268.315,204.954L268.315,204.954z"/>
                <path fill="#FFFFFF" d="M239.422,108.634c-0.003,5.154,4.255,9.334,9.513,9.339c5.259,0.005,9.527-4.169,9.528-9.323v-0.018
                    c0.006-5.154-4.254-9.336-9.512-9.338c-5.258-0.005-9.523,4.17-9.529,9.322V108.634z"/>
                <path fill="#B2B2B2" d="M248.945,119.964c-6.375,0-11.561-5.082-11.561-11.33c0-6.248,5.186-11.331,11.561-11.331
                    c6.373,0,11.559,5.083,11.559,11.331S255.319,119.964,248.945,119.964z M248.945,101.303c-4.125,0-7.481,3.29-7.481,7.333
                    c0,4.042,3.356,7.332,7.481,7.332c4.123,0,7.479-3.29,7.479-7.332C256.425,104.592,253.07,101.303,248.945,101.303z"/>
                <path fill="#B2B2B2" d="M159.156,298.707c-16.118,0-29.229-12.849-29.229-28.646s13.111-28.648,29.229-28.648
                    c16.113,0,29.224,12.852,29.224,28.648S175.271,298.707,159.156,298.707z M159.156,251.523c-10.429,0-18.916,8.316-18.916,18.54
                    c0,10.22,8.486,18.538,18.916,18.538c10.425,0,18.91-8.318,18.91-18.538C178.066,259.84,169.586,251.523,159.156,251.523z"/>
                <path fill="#FFFFFF" d="M143.652,103.525c0,3.254,2.691,5.889,6.008,5.889c3.319,0,6.008-2.635,6.008-5.889
                    c0-3.252-2.689-5.889-6.008-5.889C146.344,97.636,143.652,100.273,143.652,103.525z"/>
                <path fill="#B2B2B2" d="M149.66,110.413c-3.877,0-7.029-3.09-7.029-6.889c0-3.798,3.153-6.886,7.029-6.886s7.029,3.089,7.029,6.889
                    C156.689,107.325,153.537,110.413,149.66,110.413L149.66,110.413z M149.66,98.635c-2.751,0-4.989,2.194-4.989,4.891
                    c0,2.696,2.238,4.89,4.989,4.89c2.75,0,4.988-2.193,4.988-4.89C154.648,100.83,152.41,98.635,149.66,98.635z"/>
                <path fill="#FFFFFF" d="M179.15,123.121c-0.001,2.763,2.283,5.002,5.101,5.003s5.102-2.238,5.103-4.999v-0.005
                    c0-2.762-2.283-5.001-5.1-5.001c-2.818-0.001-5.102,2.237-5.104,4.998V123.121L179.15,123.121z"/>
                <path fill="#B2B2B2" d="M184.252,129.123c-3.375,0-6.12-2.691-6.12-6.001c0-3.308,2.747-5.999,6.12-5.999
                    c3.375,0,6.121,2.69,6.121,5.999C190.373,126.432,187.627,129.123,184.252,129.123L184.252,129.123z M184.252,119.122
                    c-2.249,0-4.081,1.794-4.081,3.999c0,2.206,1.831,4,4.081,4s4.08-1.795,4.08-4C188.332,120.917,186.502,119.122,184.252,119.122
                    L184.252,119.122z"/>
                <path fill="#B2B1B2" d="M305.959,315.85c0,2.966,2.404,5.37,5.37,5.37s5.37-2.404,5.37-5.37s-2.404-5.371-5.37-5.371
                    S305.959,312.884,305.959,315.85z M20.879,395.584H3.729c-1.279,0-2.316-1.037-2.316-2.315c0-1.279,1.037-2.316,2.316-2.316H20.88
                    c1.279,0.001,2.316,1.038,2.315,2.316C23.195,394.548,22.158,395.585,20.879,395.584L20.879,395.584z M66.425,395.584H30.653
                    c-1.279,0-2.316-1.037-2.316-2.315c0-1.279,1.037-2.316,2.316-2.316h35.773c1.279,0.01,2.308,1.055,2.298,2.334
                    C68.714,394.552,67.69,395.575,66.425,395.584L66.425,395.584z M326.271,395.584h-35.773c-1.278,0-2.315-1.037-2.315-2.315
                    c0-1.279,1.037-2.316,2.315-2.316h35.773c1.278,0,2.315,1.037,2.315,2.316C328.586,394.547,327.549,395.584,326.271,395.584z
                    M278.414,395.584H74.561c-1.279,0-2.316-1.037-2.316-2.315c0-1.279,1.037-2.316,2.316-2.316h203.855
                    c1.279,0.001,2.315,1.038,2.314,2.316C280.73,394.548,279.693,395.585,278.414,395.584z M94.982,417.679H70.956
                    c-1.279,0-2.315-1.036-2.315-2.315s1.037-2.315,2.315-2.315h24.026c1.279,0,2.316,1.036,2.316,2.315S96.261,417.679,94.982,417.679
                    z M265.482,417.679H105.406c-1.279,0-2.316-1.036-2.316-2.315s1.037-2.315,2.316-2.315h160.079c1.278,0,2.314,1.036,2.314,2.315
                    s-1.036,2.315-2.314,2.315H265.482z"/>
                <path fill="#B2B1B2" d="M219.156,181.704c0,1.529,0.939,2.77,2.1,2.77s2.1-1.241,2.1-2.77c0-1.53-0.939-2.771-2.1-2.771
                    S219.156,180.174,219.156,181.704z M107.692,222.827h-6.706c-0.5,0-0.905-0.535-0.905-1.194c0-0.66,0.405-1.195,0.905-1.195h6.706
                    c0.5,0.001,0.906,0.536,0.905,1.195C108.597,222.292,108.191,222.827,107.692,222.827L107.692,222.827z M125.5,222.827h-13.987
                    c-0.5,0-0.906-0.535-0.906-1.194c0-0.66,0.405-1.195,0.906-1.195H125.5c0.5,0.005,0.902,0.544,0.898,1.204
                    C126.395,222.294,125.995,222.822,125.5,222.827L125.5,222.827z M227.098,222.827h-13.986c-0.5,0-0.906-0.535-0.906-1.194
                    c0-0.66,0.406-1.195,0.906-1.195h13.986c0.5,0,0.905,0.535,0.905,1.195C228.003,222.292,227.598,222.827,227.098,222.827z
                    M208.386,222.827h-79.705c-0.5,0-0.905-0.535-0.905-1.194c0-0.66,0.405-1.195,0.905-1.195h79.706
                    c0.501,0.001,0.905,0.536,0.905,1.195C209.292,222.292,208.887,222.827,208.386,222.827z M136.666,234.222h-9.395
                    c-0.5,0-0.905-0.534-0.905-1.194s0.405-1.194,0.905-1.194h9.395c0.5,0,0.905,0.534,0.905,1.194S137.165,234.222,136.666,234.222z
                    M203.33,234.222h-62.589c-0.5,0-0.905-0.534-0.905-1.194s0.405-1.194,0.905-1.194h62.59c0.5,0,0.904,0.534,0.904,1.194
                    S203.831,234.222,203.33,234.222L203.33,234.222z"/>
                <path fill="#B2B1B2" d="M229.462,231.127c0,1.529,0.939,2.77,2.1,2.77s2.1-1.241,2.1-2.77c0-1.53-0.939-2.771-2.1-2.771
                    S229.462,229.597,229.462,231.127z M117.998,272.25h-6.706c-0.5,0-0.905-0.535-0.905-1.195s0.405-1.194,0.905-1.194h6.706
                    c0.5,0.001,0.906,0.536,0.905,1.194C118.903,271.715,118.497,272.25,117.998,272.25L117.998,272.25z M135.806,272.25h-13.987
                    c-0.5,0-0.906-0.535-0.906-1.195s0.405-1.194,0.906-1.194h13.987c0.5,0.005,0.902,0.544,0.898,1.204
                    C136.701,271.717,136.3,272.245,135.806,272.25L135.806,272.25z M237.403,272.25h-13.986c-0.5,0-0.906-0.535-0.906-1.195
                    s0.406-1.194,0.906-1.194h13.986c0.5,0,0.905,0.534,0.905,1.194S237.903,272.25,237.403,272.25z M218.691,272.25h-79.705
                    c-0.5,0-0.905-0.535-0.905-1.195s0.405-1.194,0.905-1.194h79.706c0.501,0.001,0.905,0.536,0.905,1.194
                    C219.598,271.715,219.192,272.25,218.691,272.25z M146.971,283.645h-9.395c-0.5,0-0.905-0.534-0.905-1.194s0.405-1.194,0.905-1.194
                    h9.395c0.5,0,0.905,0.534,0.905,1.194S147.471,283.645,146.971,283.645z M213.636,283.645h-62.589c-0.5,0-0.905-0.534-0.905-1.194
                    s0.405-1.194,0.905-1.194h62.59c0.5,0,0.904,0.534,0.904,1.194S214.137,283.645,213.636,283.645L213.636,283.645z"/>
                </svg>
            </slot>
        </div>
        <div class="v-pagination" :class="paginationClass" v-if="pagination && items.length > perPage">
            <div class="v-pagination__body">
                <div class="v-pagination-item v-first-page" @click="currentPage = 1">
                    <slot name="back-to-first">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path d="M10 6L8.59 7.41 13.17 12l-4.58 4.59L10 18l6-6z"/><path d="M0 0h24v24H0z" fill="none"/></svg><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path d="M10 6L8.59 7.41 13.17 12l-4.58 4.59L10 18l6-6z"/><path d="M0 0h24v24H0z" fill="none"/></svg>
                    </slot>
                </div>
                <div class="v-pagination-item v-prev-page" @click="prevPage()">
                    <slot name="prev">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path d="M10 6L8.59 7.41 13.17 12l-4.58 4.59L10 18l6-6z"/><path d="M0 0h24v24H0z" fill="none"/></svg>
                    </slot>
                </div>
                <div class="v-pagination-item v-page-number" :class="{'v-active-page' : currentPage ==  parseInt(page)}" v-for="(page, index) in shortPagination" :key="index" @click="page != '...' ? currentPage = parseInt(page) : ''">
                    {{page}}
                </div>
                <div class="v-pagination-item v-next-page" @click="nextPage()">
                    <slot name="next">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path d="M10 6L8.59 7.41 13.17 12l-4.58 4.59L10 18l6-6z"/><path d="M0 0h24v24H0z" fill="none"/></svg>
                    </slot>
                </div>
                <div class="v-pagination-item v-last-page" @click="lastPage()">
                    <slot name="go-to-last">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path d="M10 6L8.59 7.41 13.17 12l-4.58 4.59L10 18l6-6z"/><path d="M0 0h24v24H0z" fill="none"/></svg><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path d="M10 6L8.59 7.41 13.17 12l-4.58 4.59L10 18l6-6z"/><path d="M0 0h24v24H0z" fill="none"/></svg>
                    </slot>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {

    name: 'vuetable',
    props:{

        //basic setup

        headers:{
            type: Array,
            default: ()=>[]
        },
        items:{
            type: Array,
            default: ()=>[]
        },
        loading:{
            type: Boolean,
            default: false
        },

        // rows

        rowTemplate:{
            type: String,
            default: 'minmax(50px, 1fr)'
        },
        rowClass:{
            type: String,
            default: ''
        },
        headerClass:{
            type: String,
            default: ''
        },
        rowIds:{
            type: Boolean,
            default: false
        },

        // sort options

        sortable:{
            type: Boolean,
            default: false
        },

        //pagination

        pagination:{
            type: Boolean,
            default: false
        },
        
        perPage:{
            type: Number,
            default: 10
        },

        paginationClass:{
            type: String,
            default: ''
        },

    },
    data(){

        return{

            cells: [],
            cellsKeys:[],

            currentSort:'',
            currentSortDir: 'ASC',
            currentPage:1,

            pages: [],

        }

    },
    methods:{

        createCells(){

            for (let i = 0; i < this.headers.length; i++) {

                this.cells.push(i);
                
            }

        },

        extractKeys(){
            
            if(this.items.length > 0){

                const entries = Object.entries(this.items[0]);
                
                if(this.currentSort == '') this.currentSort = entries[0][0];
                
                for (const [key] of entries) {

                    let obj = this.headers.find(o => o.for === key);
                    
                    if(obj){

                        this.cellsKeys.push(key);

                    }

                }

            }

        },

        setColumnsNumber(){

            const template = this.rowTemplate ? this.rowTemplate : ' minmax(50px, 1fr)';

            return {
              "grid-template-columns": "repeat(" + this.cells.length + ", "+ template +")"
            }    

        },

        addId(index){

            if(this.rowIds){
                return `v-row-${index}`
            }

            return '';

        },

        sort(column) {

            if(this.sortable){

                this.currentSortDir = this.currentSortDir === 'ASC' ? 'DESC' : 'ASC';

                this.currentSort = column;

            }

        },

        countPages(){

            this.pages = [];

            let count = Math.ceil(this.items.length/this.perPage);

            for (let i = 1; i <= count; i++) {

                this.pages.push(i);
                
            }

        },

        nextPage() {

            if((this.currentPage*this.perPage) < this.items.length) this.currentPage++;

        },

        prevPage() {

            if(this.currentPage > 1) this.currentPage--;

        },

        lastPage(){

            let count = Math.ceil(this.items.length/this.perPage);
            this.currentPage = count;

        },

    },
    watch:{

        items: function(){

            this.extractKeys();
            
            if(this.pagination){

                this.countPages();

            }

        }

    },
    created(){

        this.createCells();

        if(this.pagination){

            this.countPages();

        }

    },
    computed:{

        sortedItems() {
            
            if(this.sortable && !this.pagination){
            
               return this.items.sort((a,b) => {

                    let modifier = 1;
                    if(this.currentSortDir === 'DESC') modifier = -1;
                    if(a[this.currentSort] < b[this.currentSort]) return -1 * modifier;
                    if(a[this.currentSort] > b[this.currentSort]) return 1 * modifier;
                    return 0;

                });

            }

            if(this.sortable && this.pagination){
            
               return this.items.sort((a,b) => {

                    let modifier = 1;
                    if(this.currentSortDir === 'DESC') modifier = -1;
                    if(a[this.currentSort] < b[this.currentSort]) return -1 * modifier;
                    if(a[this.currentSort] > b[this.currentSort]) return 1 * modifier;
                    return 0;

                }).filter((row, index) => {
                    let start = (this.currentPage-1)*this.perPage;
                    let end = this.currentPage*this.perPage;
                    if(index >= start && index < end) return true;
                });

            }

            if(!this.sortable && this.pagination){
            
               return this.items.filter((row, index) => {
                    let start = (this.currentPage-1)*this.perPage;
                    let end = this.currentPage*this.perPage;
                    if(index >= start && index < end) return true;
                });

            }

            if(!this.sortable && !this.pagination){
                return this.items
            }

        },

        shortPagination(){

            let delta = 1,
            pageCount = this.pages.length,
            left = this.currentPage - delta,
            right = this.currentPage + delta + 1,
            result = [];

            result = Array.from({length: pageCount}, (v, k) => k + 1)
                .filter(i => i && i >= left && i < right);

            if (result.length > 1) {
            // Add first page and dots
            if (result[0] > 1) {
                if (result[0] > 2) {
                result.unshift('...')
                }
                result.unshift(1)
            }

            // Add dots and last page
            if (result[result.length - 1] < pageCount) {
                if (result[result.length - 1] !== pageCount - 1) {
                result.push('...')
                }
                result.push(pageCount)
            }
            }
    
            return result;

        },

    },

}
</script>

<style scoped>

.v-row{
    position:relative;
    overflow: hidden;
    width: 100%;
    grid-column: 1 / -1;
    grid-gap: 1%;
    display: grid;
    background: #fff;
    padding: 15px;
    cursor: default;
    border:none !important;
    box-shadow: 0px 0px 8px 0px #d1d1d1;
    border-radius: 5px;
    margin: 5px 0px;
}

.v-row:hover{
    opacity:1;
    box-shadow: 1px 1px 8px 0px #b9b9b9;
    z-index: 1;
}

.v-row:first-child{
    border-bottom: none;
}

.v-row:nth-last-of-type(2){
    border-bottom: none;
}

.v-table{
    display: grid;
    width:100%;
}

.v-table__header{
    background:none;
    box-shadow: none;
    margin:0%;
    text-align: left;
}

.v-table-heading{
    position: relative;
}

.v-table.loading{
    opacity:0.5;
    cursor: default;
    pointer-events: none;
    transition-duration: 0.2s;
}

.v-table-data{
    display:flex;
    justify-content: space-between;
    align-items: center;
}

.none-data{
    display:flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width:100%;
}

.v-loading{
    position:absolute;
    width:100%;
    height:100%;
    top:0;
    left:0;
}

.v-pagination{
    display: flex;
    flex-direction: row-reverse;
    width:100%;
    position: relative;
    align-items: center;
}

.v-pagination__body{
    display: flex;
    position: relative;
    align-items: center;
}

.v-pagination__body .v-pagination-item{
    display:flex;
    align-items: center;
    justify-content: center;
    padding: 5px;
    cursor:pointer;
}

.v-pagination__body .v-pagination-item svg{
    width:20px;
}

.v-pagination__body .v-pagination-item:hover{
    border-bottom: 2px #ff3366 solid;
    box-sizing: border-box;
    padding-bottom: 3px;
}

.v-last-page svg:nth-child(2){
    margin-left: -10px;
}

.v-first-page svg{
    transform: rotate(180deg);
}

.v-first-page svg:nth-child(2){
    margin-left: -10px;
}

.v-prev-page svg{
    transform: rotate(180deg);
}

.v-page-number.v-active-page{
    border-bottom: 2px #ff3366 solid;
    box-sizing: border-box;
    padding-bottom: 3px; 
}

</style>
