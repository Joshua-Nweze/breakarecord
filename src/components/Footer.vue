<template>
    <footer class="mt-5">
        &copy; {{ new Date().getFullYear() }} Breakarecord <br> Designed and built by <a href="">Joshua Nweze</a>
    </footer>
</template>

<script>
export default {

}
</script>


<style>
@media only screen and (max-width: 480px)  {
    footer {
        font-size: 13px;
    }
}

a, a:hover{
    color: #42b983
}
</style>