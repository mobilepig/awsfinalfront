<template>
  <div class="container">
    <div class="bgBox">
      <div
        v-if="mode.currentBgIdx == 0"
        class="bg fadeInOut"
        :style="{
          'background-image': 'url(' + data.bgImg[0] + ')',
        }"
      >
        <div class="overlay1"></div>
        <div class="overlay2"></div>
      </div>

      <div
        v-if="mode.currentBgIdx == 1"
        class="bg fadeInOut"
        :style="{
          'background-image': 'url(' + data.bgImg[1] + ')',
        }"
      >
        <div class="overlay1"></div>
        <div class="overlay2"></div>
      </div>

      <div
        v-if="mode.currentBgIdx == 2"
        class="bg fadeInOut"
        :style="{
          'background-image': 'url(' + data.bgImg[2] + ')',
        }"
      >
        <div class="overlay1"></div>
        <div class="overlay2"></div>
      </div>

      <div
        v-if="mode.currentBgIdx == 3"
        class="bg fadeInOut"
        :style="{
          'background-image': 'url(' + data.bgImg[3] + ')',
        }"
      >
        <div class="overlay1"></div>
        <div class="overlay2"></div>
      </div>

      <div
        v-if="mode.currentBgIdx == 4"
        class="bg fadeInOut"
        :style="{
          'background-image': 'url(' + data.bgImg[4] + ')',
        }"
      >
        <div class="overlay1"></div>
        <div class="overlay2"></div>
      </div>

      <div class="highLight floating">
        <div
          class="content"
          v-on:click="
            stateFunction.goToPost(
              data.rank?.highlight[mode.highlightIndex]?.post_id
            )
          "
        >
          <div
            class="box"
            :style="{
              backgroundImage:
                'url(' +
                data.rank?.highlight[mode.highlightIndex]?.thumbnail_url +
                ')',
            }"
          >
            <div class="deco">
              <img
                v-if="
                  data.rank?.highlight[mode.highlightIndex]?.desti_type ==
                  'LANDMARK'
                "
                src="https://dgaui.s3.ap-northeast-2.amazonaws.com/destitype/Landmark.webp"
                alt=""
              />

              <img
                v-if="
                  data.rank?.highlight[mode.highlightIndex]?.desti_type ==
                  'RESTAURANT'
                "
                src="https://dgaui.s3.ap-northeast-2.amazonaws.com/destitype/Restaurant.webp"
                alt=""
              />
              <img
                v-if="
                  data.rank?.highlight[mode.highlightIndex]?.desti_type ==
                  'MARKET'
                "
                src="https://dgaui.s3.ap-northeast-2.amazonaws.com/destitype/Market.webp"
                alt=""
              />
            </div>

            <div class="info">
              <p class="poor">실시간 인기게시글</p>
              <p class="title">
                {{
                  showhighlightTitle(data.rank?.highlight, mode.highlightIndex)
                }}
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="content">
      <div class="title_again rank_title">
        <img
          src="https://dgaui.s3.ap-northeast-2.amazonaws.com/rank/title/title_again.webp"
          alt=""
        />
      </div>
      <div class="rankingBox revisit">
        <div class="rankingScroll">
          <div
            v-for="(post, idx) in data.rank.revisit"
            :key="post"
            class="card"
            style="width: 18rem"
            v-on:click="stateFunction.goToPost(post.post_id)"
          >
            <div class="thumbnail">
              <img
                v-if="modifyUrl(idx, 'again')"
                class="rankingBadge"
                :src="modifyUrl(idx, 'again')"
              />
              <img
                :src="post.thumbnail_url"
                class="card-img-top"
                alt=""
                style="height: 12rem"
              />
              <div class="mainText mainTextRevisit floating">
                <i class="fa-solid fa-arrows-rotate"></i>
                <p>
                  총 <span>{{ post.revisit_count }}</span
                  >회 방문했어요
                </p>
              </div>
            </div>

            <div class="card-body">
              <h5 class="card-title">
                {{ stateFunction.truncateText(post.title, 13) }}
              </h5>

              <div class="detail1">
                <div class="areaAndDestiBox poor">
                  <i class="fa-solid fa-map-location"></i>
                  <span>{{ stateFunction.translateArea(post.area) }}의</span>
                  <span>{{ post.desti_name }}</span>
                </div>
              </div>
              <div class="detail2">
                <div class="typeBox">
                  <p class="poor">with</p>

                  <img
                    :src="stateFunction.imgLinkKidOrElder(post.travel_type)"
                    alt=""
                  />
                </div>
                <div class="likeBox">
                  <i class="fa-solid fa-heart"></i>
                  <p>{{ post.like_count }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="title_type rank_title">
        <img
          src="https://dgaui.s3.ap-northeast-2.amazonaws.com/rank/title/title_type.webp"
          alt=""
        />
      </div>
      <ul class="nav nav-tabs">
        <li class="nav-item" v-on:click="mode.travelType = 'KID'">
          <div
            class="nav-link poor"
            :class="mode.travelType == 'KID' ? 'active' : ''"
          >
            <img
              src="https://dgaui.s3.ap-northeast-2.amazonaws.com/emoticon/KID.webp"
              alt=""
            />
            <p>아이랑 여행</p>
          </div>
        </li>
        <li class="nav-item">
          <div
            class="nav-link poor"
            :class="mode.travelType == 'ELDER' ? 'active' : ''"
            v-on:click="mode.travelType = 'ELDER'"
          >
            <img
              src="https://dgaui.s3.ap-northeast-2.amazonaws.com/emoticon/ELDER.webp"
              alt=""
            />
            <p>효도여행</p>
          </div>
        </li>
      </ul>
      <div
        class="rankingBox slideRight kidOrElder"
        v-if="mode.travelType == 'KID'"
      >
        <div class="rankingScroll">
          <div
            v-for="(post, idx) in data.rank.KID"
            :key="post"
            class="card"
            style="width: 18rem"
            v-on:click="stateFunction.goToPost(post.post_id)"
          >
            <div class="thumbnail">
              <img
                v-if="modifyUrl(idx, 'KID')"
                class="rankingBadge"
                :src="modifyUrl(idx, 'KID')"
              />
              <img
                :src="post.thumbnail_url"
                class="card-img-top"
                alt=""
                style="height: 12rem"
              />
              <div class="mainText mainTextKidOrElder floating">
                <i class="fa-solid fa-heart"></i>
                <p>{{ post.like_count }}명이 좋아합니다</p>
              </div>
            </div>

            <div class="card-body">
              <h5 class="card-title">
                {{ stateFunction.truncateText(post.title, 13) }}
              </h5>

              <div class="detail1">
                <div class="areaAndDestiBox poor">
                  <i class="fa-solid fa-map-location"></i>
                  <span>{{ stateFunction.translateArea(post.area) }}의</span>
                  <span>{{ post.desti_name }}</span>
                </div>
              </div>
              <div class="detail2">
                <div class="revisitBox poor">
                  <i class="fa-solid fa-arrows-rotate"></i>
                  <p>
                    <span>{{ post.revisit_count }}</span
                    >회 방문
                  </p>
                </div>
                <div class="likeBox"></div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div
        class="rankingBox slideRight kidOrElder"
        v-if="mode.travelType == 'ELDER'"
      >
        <div class="rankingScroll">
          <div
            v-for="(post, idx) in data.rank.ELDER"
            :key="post"
            class="card"
            style="width: 18rem"
            v-on:click="stateFunction.goToPost(post.post_id)"
          >
            <div class="thumbnail">
              <img
                v-if="modifyUrl(idx, 'ELDER')"
                class="rankingBadge"
                :src="modifyUrl(idx, 'ELDER')"
              />
              <img
                :src="post.thumbnail_url"
                class="card-img-top"
                alt=""
                style="height: 12rem"
              />
              <div class="mainText mainTextKidOrElder floating">
                <i class="fa-solid fa-heart"></i>
                <p>{{ post.like_count }}명이 좋아합니다</p>
              </div>
            </div>

            <div class="card-body">
              <h5 class="card-title">
                {{ stateFunction.truncateText(post.title, 13) }}
              </h5>

              <div class="detail1">
                <div class="areaAndDestiBox poor">
                  <i class="fa-solid fa-map-location"></i>
                  <span>{{ stateFunction.translateArea(post.area) }}의</span>
                  <span>{{ post.desti_name }}</span>
                </div>
              </div>
              <div class="detail2">
                <div class="revisitBox poor">
                  <i class="fa-solid fa-arrows-rotate"></i>
                  <p>
                    <span>{{ post.revisit_count }}</span
                    >회 방문
                  </p>
                </div>
                <div class="likeBox"></div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="title_area rank_title">
        <img
          src="https://dgaui.s3.ap-northeast-2.amazonaws.com/rank/title/title_area.webp"
          alt=""
        />
      </div>
      <div class="select">
        <div class="map">
          <img
            src="https://dgaui.s3.ap-northeast-2.amazonaws.com/leader/bg.webp"
            alt="bg"
          />
          <img
            v-for="area in mode.areaArr"
            :key="area"
            :class="mode.activeArea == area ? 'active' : 'inactive'"
            :src="getMapUrl(area)"
            :alt="area"
          />
          <img
            class="pulse object"
            src="https://dgaui.s3.ap-northeast-2.amazonaws.com/leader/objects.webp"
            alt=""
          />
        </div>
        <div class="area_btns poor" aria-label="Basic example">
          <button
            v-for="area in mode.areaArr"
            :key="area"
            v-on:click="changeModeArea(area)"
            class="btn"
            :class="area == mode.activeArea ? 'btn-primary' : 'btn-light'"
          >
            {{ stateFunction.translateArea(area) }}
          </button>
        </div>
      </div>
      <div class="rank">
        <div
          class="rankingBox slideRight areaRank"
          v-for="areaa in filteredAreas"
          :key="areaa"
        >
          <!-- rankingBox에다가 조건부로 display none  -->
          <div
            class="rankingScroll"
            :class="mode.activeArea !== areaa ? 'inactiveArea' : ''"
          >
            <div
              v-for="(post, idx) in data.rank.area[areaa]"
              :key="post"
              class="card"
              style="width: 18rem"
              v-on:click="stateFunction.goToPost(post.post_id)"
            >
              <div class="thumbnail">
                <img
                  v-if="modifyUrl(idx, 'area')"
                  class="rankingBadge"
                  :src="modifyUrl(idx, 'area')"
                />
                <img
                  :src="post.thumbnail_url"
                  class="card-img-top"
                  alt=""
                  style="height: 12rem"
                />
                <div class="mainText mainTextKidOrElder floating">
                  <i class="fa-solid fa-heart"></i>
                  <p>{{ post.like_count }}명이 좋아합니다</p>
                </div>
              </div>

              <div class="card-body">
                <h5 class="card-title">
                  {{ stateFunction.truncateText(post.title, 13) }}
                </h5>

                <div class="detail1">
                  <div class="areaAndDestiBox poor">
                    <i class="fa-solid fa-map-location"></i>
                    <span>{{ stateFunction.translateArea(post.area) }}의</span>
                    <span>{{ post.desti_name }}</span>
                  </div>
                </div>
                <div class="detail2">
                  <div class="revisitBox poor">
                    <i class="fa-solid fa-arrows-rotate"></i>
                    <p>
                      <span>{{ post.revisit_count }}</span
                      >회 방문
                    </p>
                  </div>
                  <div class="likeBox"></div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, computed } from "vue";
import axios from "axios";

export default {
  name: "Leader",
  props: {
    modalFunction: Object,
    stateFunction: Object,
    style: Object,
  },

  setup(props) {
    // const state = reactive({});
    const data = reactive({
      bgImg: [
        "https://dgafrontui.s3.ap-northeast-2.amazonaws.com/leaderboard/1.jpg",
        "https://dgafrontui.s3.ap-northeast-2.amazonaws.com/leaderboard/2.jpg",
        "https://dgafrontui.s3.ap-northeast-2.amazonaws.com/leaderboard/3.jpg",
        "https://dgafrontui.s3.ap-northeast-2.amazonaws.com/leaderboard/4.jpg",
        "https://dgafrontui.s3.ap-northeast-2.amazonaws.com/leaderboard/5.jpg",
      ],

      rank: {
        KID: [],
        ELDER: [],
        like: [],
        revisit: [],
        area: {},
        highlight: [],
      },
    });

    const filteredAreas = computed(() => {
      return mode.areaArr.filter((areaa) => mode.activeArea === areaa);
    });

    const mode = reactive({
      travelType: "KID",
      activeArea: "seoul",

      areaArr: [
        "seoul",
        "chungcheong",
        "kangwon",
        "gyeonggi",
        "jeollanam",
        "jeollabuk",
        "gyeongsangbuk",
        "gyeongsangnam",
        "jeju",
      ],
      highlightIndex: 0,
      currentBgIdx: 0,
    });

    const changeHighlight = () => {
      setInterval(() => {
        if (mode.highlightIndex == 3) {
          mode.highlightIndex = 0;
        } else {
          mode.highlightIndex++;
        }
      }, 3000);
    };

    changeHighlight();

    const modifyUrl = (idx, kind) => {
      "https://dgaui.s3.ap-northeast-2.amazonaws.com/rank/again/again1.webp";

      if (idx <= 2) {
        return `https://dgaui.s3.ap-northeast-2.amazonaws.com/rank/${kind}/${kind}${
          idx + 1
        }.webp`;
      } else {
        false;
      }
    };

    const changeModeArea = (area) => {
      mode.activeArea = area;
    };

    const bgImgInterval = () => {
      setInterval(() => {
        if (mode.currentBgIdx == data.bgImg.length - 1) {
          mode.currentBgIdx = 0;
        } else {
          mode.currentBgIdx++;
        }
      }, 5000);
    };
    bgImgInterval();

    const getData = () => {
      axios.get("/api/leaderboards/").then((res) => {
        data.rank.like = res.data.like;
        data.rank.revisit = res.data.revisit;
        data.rank.KID = res.data.KID;
        data.rank.ELDER = res.data.ELDER;
        data.rank.area = res.data.area;
        data.rank.highlight = [
          res.data.like[0],
          res.data.revisit[0],
          res.data.like[1],
          res.data.revisit[1],
        ];
      });
    };
    getData();

    props;

    const getMapUrl = (area) => {
      return `https://dgaui.s3.ap-northeast-2.amazonaws.com/leader/${area}.webp`;
    };

    const showhighlightTitle = (arr, highlightIndex) => {
      arr;
      highlightIndex;
      const title = arr?.[highlightIndex]?.title;
      const finalTitle = props.stateFunction.truncateText(
        title ? title : "waiting",
        12
      );
      return finalTitle;
    };

    return {
      data,
      mode,
      modifyUrl,
      getMapUrl,
      changeModeArea,
      filteredAreas,
      showhighlightTitle,
    };
  },
};
</script>

<style lang="scss">
#app {
  width: 100vw;
}
body {
  margin: 0;
}
.container {
  width: 100%;

  .bgBox {
    width: 100%;
    overflow: hidden;
    height: 500px;
    display: flex;
    flex-direction: column;
    align-content: center;
    justify-content: center;
    position: relative;
    .bg {
      position: absolute;
      top: 0;
      width: 100%;
      height: 500px;
      background-size: cover;
      background-position: center; /* 이미지를 가운데 정렬 */
      background-repeat: no-repeat; /* 이미지 반복 없음 */
      position: relative;
      .overlay1 {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 50%; /* 아랫부분의 높이 조정 */
        background: rgba(0, 0, 0, 0.3);
      }
      .overlay2 {
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;
        height: 50%; /* 아랫부분의 높이 조정 */
        background: linear-gradient(
          to bottom,
          rgba(0, 0, 0, 0.3),
          rgba(255, 255, 255, 1)
        );
      }
    }
    .highLight {
      align-self: center;
      position: absolute;
      bottom: -0px;
      width: 90%;
      height: 200px;

      display: flex;
      justify-content: center;
      color: white;
      background-color: white;
      border-radius: 10px;
      box-shadow: 0px 0px 20px 2vh rgba(255, 255, 255, 0.5);

      padding: 5px;
      .content {
        height: 200px;
        width: 100%;
        .box {
          border-radius: 10px;
          width: 100%;
          height: 190px;
          padding-bottom: 10px;

          background-size: cover;
          background-position: center center;
          border: white solid 2px;
          .deco {
            display: flex;
            position: absolute;
            top: -40px;
            left: -20px;
            justify-content: space-between;
            width: 95%;
            padding-top: 5px;
            img {
              width: 120px;
            }
            p {
              color: rgba(255, 255, 255, 0.8);
              margin: 0;
              font-weight: 500;
              margin-left: 10px;
            }
          }

          .info {
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            align-items: center;
            width: 100%;
            height: 100%;

            .title {
              font-size: 1.5em;
              font-weight: 700;
            }
            p {
              margin: 0;
            }
          }
        }
      }
    }
  }
  .content {
    .rank_title {
      width: 100%;
      margin-top: 10px;
      border-radius: 30px 30px 0 0;
      img {
        width: 100vw;
      }
    }
    .title_again {
      background-color: v-bind("style.colors.lightRed");
    }
    .title_type {
      background-color: v-bind("style.colors.lightYellow");
    }
    .title_area {
      background-color: v-bind("style.colors.lightBlue");
    }
    .rankingBox {
      padding: 10px;
      width: 100vw;
      overflow-x: auto;

      .rankingScroll {
        display: flex;
        gap: 20px;
        .card {
          flex: 0 0 18rem;
          box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.3);
        }
        .card-body {
          background-color: v-bind("style.colors.white2");
          padding: 0;
          h5 {
            width: 100%;
            display: flex;
            justify-content: center;
            font-weight: 700;
            padding-top: 10px;
          }
        }
        .thumbnail {
          position: relative;
          .rankingBadge {
            position: absolute;
            width: 40%;
            padding: 5px;
          }
          .mainText {
            position: absolute;
            display: flex;
            align-items: center;
            left: 0;
            bottom: 0;
            width: 100%;
            padding: 5px;
            color: white;
            font-weight: 700;
            font-size: 1.2em;
            p {
              margin: 0;
            }
          }
          .mainTextRevisit {
            i {
              margin-right: 10px;
              color: v-bind("style.colors.blue3");
              font-size: 2em;
            }
            p {
              margin: 0;
              span {
                color: v-bind("style.colors.red1");
                font-size: 1.5em;
              }
            }
          }
          .mainTextKidOrElder {
            i {
              margin-right: 10px;
              color: v-bind("style.colors.red1");
              font-size: 2em;
            }
          }
        }
      }
    }
    .revisit {
      background-color: v-bind("style.colors.lightRed");

      .rankingScroll {
        .card-body {
          .detail1 {
            width: 100%;
            display: flex;
            p {
              margin: 0;
            }
            .areaAndDestiBox {
              display: flex;
              justify-content: center;
              align-items: center;
              width: 100%;
              font-size: 1.1em;
              span {
                font-size: 1.3em;
              }

              i {
                margin-right: 7px;
                color: v-bind("style.colors.blue2");
              }
            }
          }
          .detail2 {
            display: flex;
            p {
              margin: 0;
            }
            .typeBox {
              padding-left: 10px;
              display: flex;
              justify-content: flex-start;
              align-items: center;
              width: 50%;
              font-size: 1.1em;
              p {
                margin-right: 7px;
              }
              img {
                width: 50px;
              }
            }
            .likeBox {
              padding-right: 10px;

              width: 50%;
              display: flex;
              justify-content: flex-end;
              align-items: center;
              font-size: 1.3em;
              p {
                margin: 0;
              }
              i {
                color: v-bind("style.colors.red1");
                margin-right: 5px;
              }
            }
          }
        }
        .thumbnail {
          position: relative;
          .rankingBadge {
            position: absolute;
            width: 40%;
            padding: 5px;
          }
        }
      }
    }
    .nav-tabs {
      width: 100%;
      background-color: v-bind("style.colors.lightYellow");
      color: v-bind("style.colors.white1");
      border-radius: 5px 5px 0 0;
      padding-top: 10px;

      .nav-item {
        width: 50%;
        display: flex;
        justify-content: center;
        .nav-link {
          width: 90%;
          display: flex;
          align-content: center;
          color: v-bind("style.colors.blue4");

          img {
            width: 50px;
            margin-right: 5px;
          }
          p {
            margin: 0;
            font-size: 1.1em;
            align-self: center;
          }
        }
      }

      .active {
        background-color: v-bind("style.colors.yellow1");
      }
    }
    .kidOrElder {
      background-color: v-bind("style.colors.lightYellow");

      .rankingScroll {
        .card-body {
          .detail1 {
            width: 100%;
            display: flex;
            p {
              margin: 0;
            }
            .areaAndDestiBox {
              display: flex;
              justify-content: center;
              align-items: center;
              width: 100%;
              font-size: 1.1em;
              span {
                font-size: 1.3em;
              }

              i {
                margin-right: 7px;
                color: v-bind("style.colors.blue2");
              }
            }
          }
          .detail2 {
            display: flex;
            p {
              margin: 0;
            }
            .revisitBox {
              padding-left: 10px;
              display: flex;
              justify-content: flex-start;
              align-items: center;
              width: 50%;
              font-size: 1.1em;
              p {
                margin-right: 7px;
              }
              i {
                font-size: 1.2em;
                margin-right: 5px;
                color: v-bind("style.colors.blue2");
              }
            }
            .likeBox {
              padding-right: 10px;

              width: 50%;
              display: flex;
              justify-content: flex-end;
              align-items: center;
              font-size: 1.3em;
              p {
                margin: 0;
              }
              i {
                color: v-bind("style.colors.red1");
                margin-right: 5px;
              }
            }
          }
        }
        .thumbnail {
          position: relative;
          .rankingBadge {
            position: absolute;
            width: 40%;
            padding: 5px;
          }
        }
      }
    }
    .select {
      background-color: v-bind("style.colors.blue2");
      position: relative;

      .map {
        width: 100vw;
        height: 100vw;
        position: relative;

        object {
          width: 100vw;
        }

        .inactive {
          opacity: 0.3;
        }

        img {
          position: absolute;
          width: 100%;
        }
      }
      .area_btns {
        padding: 10px;
        background-color: v-bind("style.colors.blue2");

        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        width: 100%;
        button {
          margin: 5px;
        }
      }
    }

    .rank {
      position: relative;
      .areaRank {
        background-color: v-bind("style.colors.lightBlue");
        .inactiveArea {
          padding: 0 !important;
          margin: 0 !important;
          display: none !important;
        }
        .rankingScroll {
          .card-body {
            .detail1 {
              width: 100%;
              display: flex;
              p {
                margin: 0;
              }
              .areaAndDestiBox {
                display: flex;
                justify-content: center;
                align-items: center;
                width: 100%;
                font-size: 1.1em;
                span {
                  font-size: 1.3em;
                }

                i {
                  margin-right: 7px;
                  color: v-bind("style.colors.blue2");
                }
              }
            }

            .detail2 {
              display: flex;
              p {
                margin: 0;
              }
              .revisitBox {
                padding-left: 10px;
                display: flex;
                justify-content: flex-start;
                align-items: center;
                width: 50%;
                font-size: 1.1em;
                p {
                  margin-right: 7px;
                }
                i {
                  font-size: 1.2em;
                  margin-right: 5px;
                  color: v-bind("style.colors.blue2");
                }
              }
              .likeBox {
                padding-right: 10px;

                width: 50%;
                display: flex;
                justify-content: flex-end;
                align-items: center;
                font-size: 1.3em;
                p {
                  margin: 0;
                }
                i {
                  color: v-bind("style.colors.red1");
                  margin-right: 5px;
                }
              }
            }
          }
          .thumbnail {
            position: relative;
            .rankingBadge {
              position: absolute;
              width: 40%;
              padding: 5px;
            }
          }
        }
      }
    }
  }
}
@keyframes fadeInOut {
  0% {
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
  100% {
    opacity: 1;
  }
}
</style>
