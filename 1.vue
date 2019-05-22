Vue.component('dialogue',{
    data () {
        return {

        }
    },
    template:`
    <div class="outer">
        <div class="text">
            <div>hello</div>
                <br/> 
            <div>this is a model</div>  
        </div>
        <div class="button">
            <button>取消</button>
            <button>确定</button>
        </div>
    </div>`
})
<style>
    .outer {
        width:200px;
        height:120px;
        background-color:white;
        border-radius: 8px;
        padding:10px;
    }
</style>