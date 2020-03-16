<template>
    <div class="component">
        <div class="form-group">
            <input type="text" v-model="inputTitle" placeholder="Reasonable name" />
            <input type="text" v-model="inputColor" placeholder="Any CSS color value" />
            <div class="color-box" :style="{ 'background-color': inputColor }"> </div>
            <button @click="handleAdd" :disabled="!canAdd">Add color</button>
        </div>
        <hr />

        <div class="form-group">
            <div v-for="c in list" :key="c.color" class="vertical">
                <div> {{ c.title }} </div>
                <div class="color-box" :style="{ 'background-color': c.color }"> </div>
                <button @click="remove(c.color)">Remove</button>
            </div>
        </div>
        <hr />

        <div class="import-export">
            <button @click="importLs" :disabled="!canImport">Import from web storage</button>
            <button @click="exportLs">Export to web storage</button>
            <textarea :value="imported" rows="4" cols="52"></textarea>
        </div>
    </div>
</template>

<script>
const LS_NAME = 'saved_colors';
export default {
    props: {},
    data: () => ({
        list: [],
        inputColor: '#FFFFFF',
        inputTitle: '',
        imported: '',
        backdoor: 0   // trick Vue into recomputing importLs below
    }),
    computed: {
        canAdd() {
            return !this.list.find(c => c.color == this.inputColor);
        },
        canImport() {
            this.backdoor;  // trick Vue into recomputing  this property
            let x = localStorage.getItem(LS_NAME);
            if( !x ) return false;
            try {
                x = JSON.parse(x);
                return true;
            } catch(error) {
                return false;
            }
        }
    },
    methods: {
        handleAdd() {
            if( !this.canAdd ) return;
            this.list.push({
                title: this.inputTitle ? this.inputTitle : 'Color #' + (this.list.length + 1),
                color: this.inputColor
            });
        },
        remove(color) {
            this.list = this.list.filter(c => c.color !== color);
        },
        importLs() {
            let list = localStorage.getItem(LS_NAME);
            this.list = JSON.parse(list);
            this.imported = list;
        },
        exportLs() {
            console.log('exportLs');
            localStorage.setItem(LS_NAME, JSON.stringify(this.list));
            this.backdoor++;  // trick Vue into recomputing importLs
        }
    }
}
</script>

<style scoped>
.component {

}
.form-group {
    display: inline-flex;
    flex-wrap: wrap;
}
.vertical {
    display: flex;
    text-align: center;
    flex-direction: column;
}
.form-group > * {
    margin: 0.2em 0.4em;
}
.color-box {
    min-height: 2em;
    min-width: 5em;
    border: 1px solid black;
}
.vertical .color-box {
    width: 11em;
}
.import-export button {
    margin: 0.5em;
}
.import-export textarea { margin: 0.5em; display: block; }
</style>
