<script setup>
    import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
    <header>
        <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />
        <div>
            <h1>The Imaginary Company</h1>
            <h4>Software Solutions Since 2023</h4>
        </div>
    </header>
    <div class="content-wrapper">
        <div class="wrapper">
            <HelloWorld msg="You did it!" />
        </div>
    </div>
    <div class="copyright-text">
        <p>Copyright The Imaginary Company 2023</p>
    </div>
</template>

<style scoped>

    header {
        line-height: 1.5;
        width: 100%;
        border-bottom: solid;
        padding: 20px;
        border-radius: 0px;
        border-width: thin;
        background-color: #f5f5f5;
        box-shadow: 0 5px 5px -3px rgb(0 0 0 / 16%), 0 8px 10px 1px rgb(0 0 0 / 11%), 0 3px 14px 2px rgb(0 0 0 / 10%);
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: flex-start;
    }

    .logo {
        display: block;
        rotate: 90deg;
    }

    .content-wrapper {
        background-color: white;
        border: solid;
        border-radius: 8px;
        border-width: thin;
        margin: 20px;
        margin-bottom: 0px;
        height: 100%;
        box-shadow: 0 5px 5px -3px rgb(0 0 0 / 16%), 0 8px 10px 1px rgb(0 0 0 / 11%), 0 3px 14px 2px rgb(0 0 0 / 10%);
    }

    .wrapper {
        padding: 20px;
    }

    .copyright-text {
        text-align: center;
    }

    @media (min-width: 1024px) {
        header {
            display: flex;
            place-items: center;
            padding-right: calc(var(--section-gap) / 2);
        }

        .logo {
            margin: 0 2rem 0 0;
        }

        header .wrapper {
            display: flex;
            place-items: flex-start;
            flex-wrap: wrap;
        }
    }



    @media (max-width: 600px) {
        .logo {
            height: 75px;
            width: 75px;
        }
    }
</style>
