<template>
    <div class="homeIndex">
        <!--走马灯推荐电影-->
        <div class="carousel">
            <el-carousel
                    :interval="3000"
                    type="card"
                    height="300px">
                <el-carousel-item @click="preview(index)" v-for="item in imageHotsList" :key="item.id">
                    <img :src="item.movieInfo.poster" class="carousel_pic"
                         alt="图片被外星人偷走了">
                </el-carousel-item>
            </el-carousel>
        </div>
        <!--左侧边栏-->
        <div class="mainContent">
            <!--根据影院或者影片查询-->
            <div class="leftSide">
                <!--                <h1>热门影院</h1>-->
                <!--                <el-form>-->
                <!--                    <el-form-item>-->
                <!--                        <el-input v-model="popularCinema.address" prefix-icon="el-icon-search"></el-input>-->
                <!--                    </el-form-item>-->
                <!--                </el-form>-->
                <h2 style="color: #ffb440">高分电影推荐</h2>
                <!--                电影推荐显示-->
                <div>
                    <div v-for="(item, index) in scoreArr" :key="index" class="al-box-container">
                        <div class="al-box-pretty al-box-shadow-radius">
                            <span>{{item.movie.movieName}}</span>
                            <span class="font-class">{{item.score}}</span>
                        </div>
                    </div>
                </div>
            </div>
            <div class="rightSide">
                <!--所有电影展示-->
                <el-row>
                    <el-col v-for="(item, index) in imgList" :key="index" style="width: 300px">
                        <el-card :body-style="{ padding: '0px' }"
                                 class="box-shadow box-radius m-10px">
                            {{item.poster}}
                            <img @click="toShowDetail(item.id)"
                                 style="width: 300px; height: 400px;"
                                 :src="item.movieInfo.poster" alt="">
                            <div>
                                <el-button
                                        onmouseover="this.style.backgroundColor='red';"
                                        onmouseout="this.style.backgroundColor='';"
                                        type="primary"
                                        class="al-width-100"
                                        @click="toPurchaseDetail(item.id)">
                                    选座订票
                                </el-button>
                            </div>
                        </el-card>
                    </el-col>
                </el-row>
            </div>
            <div class="clearfix"></div>
        </div>
    </div>
</template>

<script>
    import {request} from "../utils/request";

    export default {
        data() {
            return {
                url: 'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg',
                srcList: [
                    'https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg',
                    'https://fuss10.elemecdn.com/1/8e/aeffeb4de74e2fde4bd74fc7b4486jpeg.jpeg'
                ],
                currentDate: new Date(),
                popularCinema: {
                    address: '',
                }
                ,
                //热映影片图片
                imageHotsList: [],
                //所有影片图片
                imgList: [],
                scoreArr: []
            }


        },
        methods: {
            toShowDetail(mid) {
                this.$router.push({path: "showDetail/" + mid});
            },
            toPurchaseDetail(mid) {
                this.$router.push({path: "purchaseDetail/" + mid})
            },
            toHome() {
                this.$router.push({path: "home"})
            },

            preview(index) {
                this.$imagePreview({
                    images: this.imageHotsList,
                    index: index,
                })
            },


            getScore() {
                request({
                    url: 'api/movieInfo/queryOrderByScore',
                }).then(res => {
                    this.scoreArr = res.data;
                }).catch(err => {
                    console.log(err);
                });
            },
            getHots() {
                request({
                    url: 'api/order/selectHots',
                }).then(res => {
                    // console.log(res);
                    this.imageHotsList = res.data;
                    // console.log(this.imageHotsList);
                }).catch(err => {
                    console.log(err);
                });
            },
            getAllMovie() {
                request({
                    url: 'api/movie/selectAll?limit=100&offset=0',
                }).then(res => {
                    this.imgList = res.data;
                }).catch(err => {
                    console.log(err);
                });
            }
        },

        created() {
            this.getScore();
            this.getHots();
            this.getAllMovie();
        },


        mounted() {
            this.getScore();
            this.getAllMovie();
        }

    }
</script>

<style lang="scss" scoped>
    .homeIndex {
        background-color: #ffffff;
        .carousel {
            width: 70%;
            margin: 0 auto;
            height: 300px;

            .carousel_pic {
                width: 100%;
                height: 100%;
                background-position: center center;
                background-size: cover;
            }
        }

        .mainContent {
            width: 100%;
            margin-top: 50px;

            .leftSide {
                padding: 20px 0 0 20px;
                width: 290px;
                float: left;
                /*background-color: #fffdf5;*/
                border: black;
                border-radius: 20px;
            }

            .rightSide {
                padding: 20px 20px;
                width: 75vw;
                float: right;
            }
        }
    }
    .el-carousel__item h3 {
        color: #475669;
        font-size: 14px;
        opacity: 0.75;
        line-height: 200px;
        margin: 0;
    }

    .el-carousel__item:nth-child(2n) {
        background-color: #99a9bf;
    }

    .el-carousel__item:nth-child(2n+1) {
        background-color: #d3dce6;
    }

    .el-carousel__item--card {
        width: 30% !important;
    }

    .font-class {
        color: #FFC600;
        font-style: oblique;
        float: right;

    }
</style>
<style lang="scss">
    .homeIndex {
        .el-carousel__container {
            margin-left: 130px !important;
        }
    }
</style>