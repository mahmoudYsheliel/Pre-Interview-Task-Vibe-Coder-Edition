<script setup lang="ts">
import Button from 'primevue/button';
import InputText from 'primevue/inputtext';
import { onMounted, ref } from 'vue';

const APIKey = ref('')



const industry = ref('')
const trend = ref('')
const idea = ref('')

onMounted(() => {
    window.addEventListener("load", () => {
        setTimeout(() => {
            document.getElementById("welcome").style.display = "none";
            document.getElementById("content").style.display = "block";
        }, 2000);
    });

})
function animateIdea(){
    if (APIKey.value == '')
        idea.value = 'Please Add API Key'
    else if (industry.value == '')
        idea.value = 'Missing Industry'
    else if (trend.value == '')
        idea.value = 'Missing Trend'
    else{
        let response_val = '' 
    fetch("https://api.openai.com/v1/chat/completions", {
    method: "POST",
    headers: {
      "Authorization": `Bearer ${APIKey.value}`,
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      model: "gpt-3.5-turbo",
      messages: [{ role: "user", content: 'I will provide you with industry and trend and you give me startup idea and a short one-liner pitch, industry' + industry.value + 'trend' + trend.value }]
    }),
  }).then( async (res)=>{
    const data = await res.json() 
    response_val =data?.choices?.[0]?.message?.content || 'No response'
  })
    
    document.getElementById("Ptrend").style.display = "block";
    document.getElementById("Pindustry").style.display = "block";
    document.getElementById("Ptrend").classList.add('Ptrend')
    document.getElementById("Pindustry").classList.add('Pindustry')
    document.getElementById("center-box").classList.remove('animate-box')
    setTimeout(()=>{
        document.getElementById("Ptrend").style.display = "none";
        document.getElementById("Pindustry").style.display = "none";
        document.getElementById("center-box").classList.add('animate-box')
        idea.value = response_val
    },2000)
    }


}



</script>

<template>
    <main>
        <h1 id="welcome">Welcome To Startup Idea Generator</h1>
        <div id="content" style="display: none;">
            <h1 id="title">Startup Idea Generator</h1>
            <div class="inputs">
                <div class="input">
                    <h3>Industry</h3>
                    <InputText v-model="industry" />
                    <p style="display: none;z-index: 2;" class="Pindustry" id="Pindustry">{{ industry }}</p>
                </div>
                <div class="input">
                    <h3>API Key</h3>
                    <InputText v-model="APIKey" />
                </div>
                <div class="input">
                    <h3>Trend</h3>
                    <InputText v-model="trend" />
                    <p style="display: none;z-index: 2;" class="Ptrend"  id="Ptrend"> {{ trend }}</p>
                </div>
            </div>
            <div style="margin-inline:auto ; width: fit-content; margin-top: 16px;">
                <Button label="Generate Idea" @click="animateIdea" />
            </div>

            <div id="center-box"> {{ idea }}</div>
        </div>

    </main>

</template>

<style scoped>
.Ptrend {
    animation: animateTrend 2s forwards;
}
.Pindustry {
    animation: animateIndustry 2s forwards;
}
@keyframes animateIndustry{
    0%{
      transform: translate(0,0);
    }
    50%{
        transform: translate(0,200px);
    }
    100%{
        transform: translate(calc(45vw - 60px),200px);
    }
}
@keyframes animateTrend{
    0%{
        transform: translate(0,0);
    }
    50%{
        transform: translate(0,200px);
    }
    100%{
        transform: translate(calc(-45vw + 60px),200px);
    }
}


#center-box {
    margin-inline: auto;
    display: block;
    width: 60%;
    min-height: 20vh;
    text-align: center;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: black;
    border-radius: 8px;
    margin-top: 16px;
    box-shadow: rgba(149, 157, 165, 0.2) 0px 8px 24px;
    font-size: 20px;
    padding: 12px;
    
}

.animate-box{
    animation:animateBox 2s forwards;
}
@keyframes animateBox{
    0%{
        transform: rotateY(0)
    }
   
    100%{
        transform: rotateY(360deg)
    }
}


.input {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.inputs {
    display: flex;
    width: 90%;
    margin-inline: auto;
    justify-content: space-between;
    height: 120px;
}

input {
    width: 100px;
}

#title {
    text-align: center;
    margin-top: 10vh;
}

#welcome {
    position: absolute;
    top: 30%;
    left: 50%;
    transform: translateX(-50%);
    animation: welcomAnimation 2s forwards;
    text-align: center;
}


@keyframes welcomAnimation {
    0% {
        top: 50%;
        opacity: 0;
    }

    37.5% {
        top: 30%;
        opacity: 1;
    }

    62.5% {
        top: 30%;
        opacity: 1;
    }

    100% {
        top: 20%;
        opacity: 0;
    }
}
*{
    margin: 0;
    padding: 0;
}
</style>
