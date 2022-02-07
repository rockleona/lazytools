<script>
// import hash from 'object-hash';
import {sha1, sha256, sha384, sha512} from 'crypto-hash';

const hashAlgo = [
    {name:'sha1', active: true},
    {name:'sha256', active: false},
    {name:'sha384', active: false},
    {name:'sha512', active: false},
]

async function hashCatcher(text, type){
    var hashingValue;
    switch (type) {
        case 'sha1':
            hashingValue = await sha1(text);
            break;
        case 'sha256':
            hashingValue = await sha256(text);
            break;
        case 'sha384':
            hashingValue = await sha384(text);
            break;
        case 'sha512':
            hashingValue = await sha512(text);
            break;
        default:
            break;
    }
    console.log(hashingValue);
    return hashingValue
}

export default {
    methods:{
        algoChange(event){
            this.hashAlgo.forEach(element => {
                if(element.name != event){
                    element.active = false;
                }else{
                    element.active = true;
                    this.hashAlgoName = event;
                }
            });
        },
        copyHash(){
            navigator.clipboard.writeText(this.hashOutput);
        }
    },
    watch:{
        plainInput(val, oldVal){
            hashCatcher(val, this.hashAlgoName).then((result)=>{
                this.hashOutput = result;
            });
        },
    },
    data(){
        return{
            hashAlgo,
            hashAlgoName:"sha1",
            plainInput:"",
            hashOutput:""
        }
    },
}
</script>

<template>
    <div class="container bg-black mx-auto px-5 py-5 font-mono text-white">
        <div class="text-center">
            <p class="text-2xl my-3">Hash Options:</p>
            <div class="gap-3 mx-auto justify-center flex">
                <button v-for="item in hashAlgo" @click="algoChange(item.name)" :key="item.name" type="button" :class="[item.active ? 'bg-secondary-yellow text-secondary-grey shadow-secondary-yellow/75':'bg-primary', 'w-20 rounded-full shadow-lg']">{{item.name}}</button>
            </div>
        </div>

        <div class="mx-auto my-10 w-full flex flex-col justify-center">
            <div class="mx-auto mb-4 px-2 w-1/2">
                <label class="block mb-1 text-sm" for="plain">Plain: </label>
                <input id="plain" class="text-black w-full border px-4 py-2 rounded focus:border-blue-500 focus:shadow-outline outline-none" type="text" placeholder="Input Plaintext here...." v-model="plainInput"/>
            </div>
            <div class="mx-auto mb-4 px-2 w-1/2">
                <label class="block mb-1 text-sm" for="hash">Hash:</label>
                <input id="hash" class="text-black w-full border px-4 py-2 rounded focus:border-blue-500 focus:shadow-outline outline-none" type="text" placeholder="Not yet hash" v-model="hashOutput" readonly/>
                <div class="flex justify-center">
                    <button class="my-5 w-1/2 rounded text-white bg-secondary-yellow hover:bg-yellow-200 hover:text-black" @click="copyHash">Copy Hash</button>
                </div>
            </div>
        </div>
    </div>
</template>
