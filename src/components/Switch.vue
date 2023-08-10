<template>
    <svg class="modeButton" :style="{ fill : darkMode ? 'black' : 'white'}" width="24" height="24" viewBox="0 0 24 24" fill="none">
        <g clip-path="url(#clip0_167_2)">
            <path d="M12 0C5.37333 0 0 5.37333 0 12C0 18.6267 5.37333 24 12 24C18.6267 24 24 18.6267 24 12C24 11.3867 23.9467 10.7733 23.8667 10.1867C22.56 12.0133 20.4267 13.2 18 13.2C14.0267 13.2 10.8 9.97333 10.8 6C10.8 3.58667 11.9867 1.44 13.8133 0.133333C13.2267 0.0533332 12.6133 0 12 0Z"/>
        </g>
        <defs>
        <clipPath id="clip0_167_2">
            <rect width="24" height="24" fill="white"/>
        </clipPath>
        </defs>
    </svg>
</template>

<script>
    export default{
        props: {
            darkMode: Boolean
        }
    }
</script>

<style>
    .modeButton{
        width: 14px;
        height: 14px;
        padding: 7.5px;
        opacity: .25;
        border-radius: 90em;
        margin-right: auto;
        margin-left: 6px;
        transition: .2s;
        cursor: pointer;
    }

    .modeButton:hover{
        opacity: .4;
    }

    .modeButton:active{
        opacity: .5;
    }
</style>