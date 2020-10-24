<template>
<!-- component -->
<div class="p-5 flex flex-wrap items-center justify-center">
    <div class="gfg shadow-2xl h-48 " ref="printMe">
        <div class="show_bg_2 rounded"></div>
        <!-- <img src="https://source.unsplash.com/featured/?white color" class="image w-full h-full rounded">
       -->
        <h3 class="first-txt text-white font-bold font-mono space-x-1">
            {{quote}} <br> - {{byline}}
        </h3>
        <h3 class="second-txt text-white font-bold font-mono pt-10">

        </h3>
    </div>
    <!--   <h3 class="second-txt">
            A computer science portal
        </h3>
   <img :src="output">
    <button @click="print">Ss</button>-->

</div>
</template>

<script>
import axios from "axios";
export default {
    data() {
        return {
            byline: null,
            quote: null,
            output: null,
        };
    },
    methods: {
        async print() {
            const el = this.$refs.printMe;
            // add option type to get the image version
            // if not provided the promise will return
            // the canvas.
            const options = {
                type: "dataURL",
            };
            this.output = await this.$html2canvas(el, options);
        },
    },
    async asyncData({
        isDev,
        route,
        store,
        env,
        params,
        query,
        req,
        res,
        redirect,
        error,
    }) {
        const quotes = await axios.get(
            `https://indie-hackers.firebaseio.com/loadingQuotes/.json`
        );
        // return {
        //     quotesList: quotes,
        // };
        console.log(quotes.data.length);
        var num = Math.floor(Math.random() * quotes.data.length) + 1;
        const quote = await axios.get(
            `https://indie-hackers.firebaseio.com/loadingQuotes/${num}.json`
        );
        console.log(quote.data);
        return {
            byline: quote.data.byline,
            quote: quote.data.quote,
        };
    },
};
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
@apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
.gfg {
    margin: 3%;
    position: relative;
    width: 23rem;
}

.show_bg_2 {
    background-image: linear-gradient(to bottom,
            rgba(245, 246, 252, 0.52),
            rgba(59, 59, 59, 0.73)),
        url("https://source.unsplash.com/featured/?natural,building");
    width: 100%;
    height: 100%;
    background-size: cover;
    color: white;
    padding: 20px;
}

.first-txt {
    position: absolute;
    top: 30px;
    left: 4%;
    margin: revert;
}

.second-txt {
    position: absolute;
    bottom: 20px;
    top: 55px;
    left: 60px;
}
</style>
