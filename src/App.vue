<template>
  <header class="header">
    <div class="container">
      <div class="header__row">
        <div class="burger" @click="openMenu"></div>
        <div class="burger__menu">
          <nav class="nav">
            <div class="nav__list">
              <div class="nav__item">Контакты</div>
              <div class="nav__item">Доставка</div>
              <div class="nav__item">Оплата</div>
              <div class="nav__item">Контакты</div>
              <div class="nav__item">О&nbspкомпании</div>
            </div>
          </nav>
        </div>
        <nav class="nav">
          <div class="nav__list">
            <div class="nav__item">Контакты</div>
            <div class="nav__item">Доставка</div>
            <div class="nav__item">Оплата</div>
            <div class="nav__item">Контакты</div>
            <div class="nav__item">О&nbspкомпании</div>
          </div>
        </nav>
        <div class="search">
          <input type="search" v-model="search" class="search__input" placeholder="Поиск по названию картины">
          <div class="button search__button">Найти</div>
        </div>
      </div>
    </div>
  </header>
  <main class="main">
    <div class="container">
      <h1 class="main__title">Картины эпохи Возрождения</h1>
      <div class="main__row">
        <Card v-for="post in filteredList" v-bind:post="post" @buy="buy"/>
      </div>
    </div>
  </main>
  <Footer/>
</template>

<script>
import Footer from './components/Footer';
import Card from './components/Card';


export default {
  name: 'App',
  data() {
    return {
      search: '',
      post: {
        type: Object,
        required: true,
      },
      postList: [
        {
          img: 'https://cs9.pikabu.ru/post_img/big/2019/05/01/6/1556701613241163083.jpg',
          title: '«Рождение Венеры» Сандро Боттичелли',
          price: '1 000 000 $',
          sale: '2 000 000 $',
          slide1: 'https://n1s1.hsmedia.ru/a1/b2/6e/a1b26e69e5296fe9cd59242776a24ea2/3714x1962_0xac120003_17843692591629489305.jpg',
          slide2: 'https://ir.ozone.ru/s3/multimedia-s/c1000/6020293444.jpg',
        },
        {
          img: 'https://planetofhotels.com/guide/sites/default/files/styles/paragraph__hero_banner__hb_image__1880bp/public/hero_banner/Mosaic-of-Last-supper-of-Jesus.jpg',
          title: '«Тайная вечеря»  Леонардо да Винчи',
          price: '3 000 000 $',
          slide1: 'https://япос.рф/upload/iblock/ed9/95c3863b901f9614c5ffd16a40f46439.jpg',
          slide2: 'https://italiamilanotour.com/wp-content/uploads/2020/04/tajnaja-vecherja-leonardo-da-vinchi-i-cerkov-marija-della-gracie-01.jpg',
        },
        {
          img: 'https://n1s1.hsmedia.ru/7a/33/ce/7a33ce01bccadb2679dde1f932f22a80/690x380_0x0a330c2a_48643381608556039.jpeg',
          title: '«Сотворение Адама» Микеланджело',
          price: '5 000 000 $',
          sale: '6 000 000 $',
          slide1: 'https://avatars.mds.yandex.net/get-mpic/4420830/img_id4944321220846543068.jpeg/orig',
          slide2: 'https://viarcanvas.com/storage/gallery-items/November2020/xFbwv8UKBQgua4VPBwB9.jpg',
        },
        {
          img: 'https://upload.wikimedia.org/wikipedia/commons/thumb/4/4d/Rembrandt_-_The_Anatomy_Lesson_of_Dr_Nicolaes_Tulp.jpg/350px-Rembrandt_-_The_Anatomy_Lesson_of_Dr_Nicolaes_Tulp.jpg',
          title: '«Урок анатомии»  Рембрандт',
          price: 'Продана на аукционе',
          slide1: 'https://slonimsmc.grodno.by/museum/Histori%20med/Razdel4_anatom/images/kar%203.jpg',
          slide2: 'https://img-fotki.yandex.ru/get/6622/125705038.22f/0_15920d_63c2a23e_orig.jpg',
        },
      ],
    };
  },
  components: {
    Footer,
    Card
  },
  mounted() {
    window.addEventListener('load', () => {
      const buttonIndex = JSON.parse(sessionStorage.getItem('buttonIndex'));
      const buyButtons = document.querySelectorAll('.main__button');

      console.log(this.post);

      buyButtons[buttonIndex].classList.add('main__button_cart');
      buyButtons[buttonIndex].innerHTML = ' <svg width="30" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">' +
          '<path d="M16.5316 4.80934L7.63353 14.2369L3.34826 10.1923" stroke="#F4F6F9" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>' +
          '</svg> В корзине';
    });
  },
  computed: {
    filteredList() {
      return this.postList.filter(post => {
        return post.title.toLowerCase().includes(this.search.toLowerCase());
      });
    }
  },
  methods: {
    buy() {
      const buyButtons = document.querySelectorAll('.main__button');

      buyButtons.forEach((item, index) => {
        item.addEventListener('click', () => {
          item.disabled = true;
          item.innerHTML = ' <div class="spinner-border" role="status"></div>' +
              '<span class="processed_text">Обработка</span>'
          ;
          item.classList.add('main__button_processed');

          setTimeout(() => {
            item.innerHTML = ' <svg width="30" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">' +
                '<path d="M16.5316 4.80934L7.63353 14.2369L3.34826 10.1923" stroke="#F4F6F9" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>' +
                '</svg> В корзине';
            item.classList.add('main__button_cart');
            sessionStorage.setItem('buttonIndex', JSON.stringify(index));
          }, 2000);
        });
      });
    },
    openMenu() {
      document.querySelector('.burger').classList.toggle('burger_active');
      document.querySelector('.burger__menu').classList.toggle('burger__menu_active');
    }
  }
};
</script>

<style lang="scss">
#app {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.spinner-border {
  width: 15px;
  height: 15px;
  position: absolute;
  left: 8px;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  height: 100vh;
}

body {
  color: #343030;
  font-family: "Merriweather", serif;
  line-height: 150%;
}

p {
  margin: 0;
}

.header, .footer {
  flex: 0 0 auto;
}

.main {
  flex: 1 0 auto;
}

.button {
  background: #403432;
  color: #ffffff;
  display: block;
  user-select: none;
  cursor: pointer;
  transition: background .3s;
  font-size: 14px;

  &:hover {
    background: #776763;
  }
}

.container {
  max-width: 1200px;
  padding: 0 10px;

  @media (min-width: 576px) {
    padding: 0 20px;
  }
}

.header {
  .nav {
    display: none;

    @media (min-width: 959px) {
      display: block;
    }
  }

  .burger {
    display: block;
    position: relative;
    z-index: 12;
    cursor: pointer;
    width: 40px;
    height: 1rem;
    left: 10px;

    @media (min-width: 959px) {
      display: none;
    }

    &::before,
    &::after {
      left: 0;
      position: absolute;
      height: 19%;
      width: 100%;
      transition: all .3s ease;
      transform-origin: center;
      background-color: #343030;
      content: "";
    }

    &::before {
      top: 0;
    }

    &::after {
      bottom: 0;
    }

    &_active {
      &::before {
        top: 50%;
        transform: rotate(-45deg) translateY(-50%);
      }

      &::after {
        bottom: 50%;
        transform: rotate(45deg) translateY(50%);
      }
    }
  }

  .burger__menu {
    position: fixed;
    top: 0;
    left: 100%;
    background: #fff;
    width: 100vw;
    height: 100vh;
    z-index: 11;
    display: flex;
    flex-direction: column;
    transition: all .4s ease;
    overflow-x: hidden;
    align-items: center;

    &_active {
      left: 0;
    }

    .nav {
      display: flex;
      align-items: center;

      &__item {
        text-align: center;
        font-size: 27px;
        margin-bottom: 15px;
      }
    }
  }
}

.nav {
  flex: 1;

  @media (min-width: 959px) {
    margin-left: 72px;
  }

  &__list {
    display: flex;
    flex-direction: column;

    @media (min-width: 959px) {
      flex-direction: row;
    }
  }

  &__item {
    margin-right: 24px;
    margin-left: 24px;
    font-size: 17px;
    line-height: 150%;
    cursor: pointer;
    margin-bottom: 7px;

    @media (min-width: 959px) {
      font-size: 14px;
    }
  }
}

.header {
  height: 97px;
  border-bottom: 1px solid #E1E1E1;
  display: flex;
  align-items: center;

  &__row {
    display: flex;
    align-items: center;
  }

  .search {
    margin-left: auto;
    margin-right: auto;
    display: flex;
    width: 60%;

    @media (min-width: 959px) {
      font-size: 14px;
      margin-right: unset;
      max-width: 416px;
    }

    &__input {
      width: 100%;
      display: block;
      padding: 13px 15px 14px 15px;
      font-size: 12px;
      outline: unset;
      border-radius: 0;
      border-bottom: 1px solid #9F9F9F;
      border-top: 1px solid #9F9F9F;
      border-left: 1px solid #9F9F9F;
      border-right: none;

      @media (min-width: 959px) {
        font-size: 14px;
      }

      &::placeholder {
        color: #9F9F9F;
        width: 100%;
      }

      &::-webkit-search-cancel-button {
        display: none;
      }

      &::-ms-clear {
        display: none;
      }
    }

    &__button {
      padding: 13px 36px;
    }
  }
}

.main {
  &__title {
    font-weight: 700;
    font-size: 24px;
    margin-top: 45px;
    margin-bottom: 39px;
    display: flex;
    justify-content: center;

    @media (min-width: 959px) {
      justify-content: flex-start;
    }
  }

  &__row {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;

    @media (min-width: 659px) {
      justify-content: space-around;
    }
    @media (min-width: 959px) {
      justify-content: space-between;
    }
  }

  &__card {
    width: 280px;
    border: 1px solid #E7E7E7;
    margin: 0 2px 30px;

    &:nth-child(4) {
      opacity: .5;

      .main__subtitle {
        margin-bottom: 34px;
      }

      .main__button {
        display: none;
      }
    }
  }

  &__image {
    position: relative;
    overflow: hidden;
    padding-bottom: 57%;
    cursor: pointer;
    width: 280px;

    img {
      object-fit: cover;
      width: 100%;
      height: 100%;
      position: absolute;
    }
  }

  &__subtitle {
    margin-bottom: 22px;
    font-size: 18px;
    cursor: pointer;
  }

  &__sale {
    text-decoration: line-through;
    opacity: .5;
    font-size: 12px;

    @media (min-width: 659px) {
      font-size: 14px;
    }
  }

  &__price {
    font-weight: 700;
    font-size: 14px;

    @media (min-width: 659px) {
      font-size: 16px;
    }
  }

  &__group {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  &__button {
    width: 118px;
    height: 48px;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;

    &_processed {
      background: #C1B4B1;
      font-size: 13px;
    }

    &_cart {
      background: #5B3A32;
    }
  }

  .processed_text {
    margin-left: 10px;
  }

  &__content {
    padding: 13px 15px;

    @media (min-width: 659px) {
      padding: 20px 24px;
    }
  }

  .dialog {
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(67, 61, 57, 0.1);
    position: fixed;
    z-index: 2;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow-x: hidden;
  }

  .popup {
    display: flex;
    background: #ffffff;
    position: relative;
    padding: 10px 20px;

    @media (min-width: 959px) {
      padding: 30px 60px;
    }

    .main__subtitle {
      cursor: unset;
    }

    &__content {
      margin-left: 30px;
    }

    &__close {
      position: absolute;
      right: 10px;
      top: 10px;
      cursor: pointer;
      border: none;
      background: none;
      z-index: 10;
    }
  }
}

</style>