<template>
    <div>
        <table :class="tableClass">
            <thead>
                <tr>
                    <slot v-for="c in columns" :name="'table-header-' + c.field">
                        <th :class="c.class" @click="toggleSort(c)">
                            <div class="column-controls">
                                <span>{{ c.label }}</span>
                                <span class="icon is-small" v-if="c.sortable">
                                    <i v-if="sort.column == c.field && sort.desc" class="fa fa-sort-desc"></i>
                                    <i v-else-if="sort.column == c.field && !sort.desc" class="fa fa-sort-asc"></i>
                                    <i class="fa fa-sort" v-else></i>
                                </span>
                            </div>
                        </th>
                    </slot>
                </tr>
            </thead>
            <tbody>
                <tr v-if="filterRow">
                    <td v-for="c in columns" v-if="c.filterable">
                        <input type="text" class="input" :placeholder="'Filter ' + c.label" @input="filterData(c, $event.target.value.toUpperCase())">
                    </td>
                    <td v-else></td>
                </tr>
                <tr v-for="row in showedRows" @click="rowClick(row)">
                    <slot name="table-body" :row="row">
                        <td v-for="c in columns"> {{ row[c.field] }} </td>
                    </slot>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default{
        props:{
            rows: {
                type: Array,
                required: true
            },
            columns: {
                type: Array,
                required: true
            },
            tableClass: {
                type: String,
                required: false,
                default: 'table'
            },
            click: {
                required: false,
                type: Boolean,
                default: false
            }
        },
        data: function(){
            return {
                sort: {
                    column: '',
                    desc: true
                },
                showedRows: []
            }
        },
        watch: {
            rows: function(){
                this.showedRows = JSON.parse(JSON.stringify(this.rows));
            }
        },
        mounted: function () {
            this.showedRows = JSON.parse(JSON.stringify(this.rows));
        },
        computed: {
            filterRow: function () {
                return this.columns.findIndex( (e) => e.filterable ) >= 0;
            }
        },
        methods:{
            rowClick: function (row) {
                if (this.click) this.$emit('row-click', row);
            },
            toggleSort: function(column){
                if(column.sortable){
                    if (column.field === this.sort.column) {
                        this.sort.desc = !this.sort.desc;
                    } else {
                        this.sort.column = column.field;
                        this.sort.desc = true;
                    }
                    this.sortData();
                }
            },
            sortData() {
                this.showedRows = this.showedRows.sort(
                    (a, b) => {
                        if(a[this.sort.column] > b[this.sort.column]){
                            return 1;
                        }
                        else if(a[this.sort.column] < b[this.sort.column]){
                            return -1;
                        }
                        return 0;
                    }
                );

                if (!this.sort.desc) {
                    this.showedRows.reverse();
                }
            },
            filterData(column, query) {
                if (column === undefined || this.rows === undefined || query === undefined || query === "") {
                    this.showedRows = this.rows;
                    return;
                }

                this.showedRows = [];
                this.rows.forEach( (row) => {
                    let search = query.toUpperCase().trim();
                    if (row[column.field].toUpperCase().indexOf(search) !== -1) {
                        this.showedRows.push(row);
                    }
                });

            }
        }
    }

</script>