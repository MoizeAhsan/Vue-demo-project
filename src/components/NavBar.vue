<template>
    <div>
        <b-navbar type='dark' variant='dark'>
            <b-navbar-nav>
                <b-nav-item href='#'>
                    Home
                </b-nav-item>
                <b-nav-item disabled>
                    Demo App
                </b-nav-item>
                <b-nav-text>
                    Question: {{x}} of {{y}}
                </b-nav-text>
            </b-navbar-nav>
        </b-navbar>
        <br>
    </div>
</template>
<script>
export default {
    props:{
        x:Number,
        y:Number
    }
}
</script>
<style scoped>
.right{
    align-content: flex-end;
}
</style>

