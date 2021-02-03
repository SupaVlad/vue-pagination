<template>
    <div class="footer">
        <div class="container">
            <span class="footer-title">Контакты</span>
            <a class="footer-tel" href="tel:+74956416155">+7(495)-641-61-55</a>
            <span class="footer-sheduel">Пн-Пт с 09:00 до 17:00 Сб-Вс: выходной</span>
            <div class="footer-line">
                <span>Интернет магазин | Все права защищены © 2020</span>
            </div>
        </div>
    </div>
</template>
<script>
    export default {
        name: "",
        data: () => ({}),
    }
</script>

<style>
    .footer{
        display: block;
        position: relative;
        padding: 81px 0 120px 0;
        margin-top: 65px;
        background: #F6F6F6;
        text-align: center;
    }
    .footer-title{
        display: block;
        margin-bottom: 26px;
        font-style: normal;
        font-weight: 600;
        font-size: 16px;
        line-height: 20px;
        text-transform: uppercase;
        color: #262329;
    }
    .footer-tel{
        display: flex;
        justify-content: center;
        font-style: normal;
        font-weight: normal;
        font-size: 14px;
        line-height: 17px;
        color: #222222;
        text-decoration: none;
    }
    .footer-tel:before{
        content: url("~@/assets/tel.svg");
        margin-right: 5px;
        margin-bottom: 23px;
    }
    .footer-sheduel{
        display: flex;
        justify-content: center;
        font-style: normal;
        font-weight: normal;
        font-size: 14px;
        line-height: 17px;
        color: #222222;
    }
    .footer-sheduel:before{
        content: url("~@/assets/time.svg");
        margin-right: 5px;
        margin-bottom: 23px;
    }
    .footer-line{
        position: absolute;
        background: #222222;
        width: 100%;
        bottom: 0;
        padding: 7px 0;
        font-style: normal;
        font-weight: normal;
        font-size: 12px;
        line-height: 15px;
        color: #B9B9B9;
    }
</style>
