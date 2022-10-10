<template>
    <div class="useInfo">
        <div id="output">
            <div
                class="main"
                :style="{
                    backgroundImage: `url(${characters[0].image})`,
                    minHeight: '500px',
                }"
            >
                <div class="world">
                    <n-h3><n-text class="uid">UID 122234</n-text></n-h3>
                    <n-h1 prefix="bar">
                        <n-text type="primary" class="ts"> 世界探索 </n-text>
                    </n-h1>
                </div>
                <div class="stats">
                    <n-h3 class="item">
                        <n-text tag="div">活跃天数</n-text>
                        <n-text tag="div" class="value">{{ value.data.stats.active_day_number }}</n-text>
                    </n-h3>
                    <n-h3 class="item">
                        <n-text tag="div">成就解锁</n-text>
                        <n-text tag="div" class="value">{{ value.data.stats.achievement_number }}</n-text>
                    </n-h3>
                    <n-h3 class="item">
                        <n-text tag="div">深境螺旋</n-text>
                        <n-text tag="div" class="value">{{ value.data.stats.spiral_abyss }}</n-text>
                    </n-h3>
                    <n-h3 class="item">
                        <n-text tag="div">获得角色</n-text>
                        <n-text tag="div" class="value">{{ value.data.stats.avatar_number }}</n-text>
                    </n-h3>
                </div>
            </div>
            <div class="avatars">
                <n-grid :x-gap="12" :y-gap="8" :cols="4">
                    <n-gi
                        class="item"
                        v-for="(item, index) in characters"
                        :key="index"
                        :class="{ four: item.rarity === 4, five: item.rarity === 5 }"
                    >
                        <img class="icon" :src="item.icon" />
                        <n-text
                            tag="div"
                            class="fetter"
                            :style="{
                                background: fetterColors[item.fetter],
                            }"
                            >好感{{ item.fetter }}</n-text
                        >
                        <div class="level">
                            Lv{{ item.level }}
                            <n-text>{{ item.constellations.filter((item) => item.is_actived).length }}命</n-text>
                        </div>
                        <div class="weapon">
                            <img
                                :src="item.weapon.icon"
                                :style="{ background: weaponRarityColors[item.weapon.rarity] }"
                            />
                            <div class="des">
                                <n-text tag="div" class="lv">Lv{{ item.weapon.level }}</n-text>
                                <n-text tag="div" class="af">精炼{{ item.weapon.affix_level }}</n-text>
                            </div>
                        </div>
                    </n-gi>
                </n-grid>
            </div>
        </div>
        <div id="input">
            <n-input
                :default-value="valueDisplay"
                type="textarea"
                placeholder="plz input value"
                :autosize="{
                    minRows: 4,
                }"
                @change="handleChange"
            ></n-input>
        </div>
    </div>
</template>

<script lang="ts">
import { computed, defineComponent, ref } from 'vue';
import { NButton, NInput, NGradientText, NText, NH1, NH3, useMessage, NGrid, NGi } from 'naive-ui';
import { userDataSample } from './data';
import { debounce } from 'lodash';

export default defineComponent({
    components: {
        NButton,
        NInput,
        NGradientText,
        NText,
        NH1,
        NH3,
        NGrid,
        NGi,
    },
    setup() {
        const message = useMessage();
        const value = ref(userDataSample);
        const valueDisplay = computed(() => JSON.stringify(value.value));
        const characters = computed(() => {
            value.value.data.avatars
                .sort((a, b) => {
                    if (a.actived_constellation_num > b.actived_constellation_num) return -1;
                    return 1;
                })
                .sort((a, b) => {
                    if (a.fetter > b.fetter) return -1;
                    return 1;
                })
                .sort((a, b) => {
                    if (a.rarity > b.rarity) return -1;
                    return 1;
                });
            return value.value.data.avatars;
        });
        console.log(characters);
        const handleChange = debounce((v: string) => {
            try {
                value.value = JSON.parse(v);
            } catch (error) {
                message.error('invalid json');
            }
        });
        const fetterColors = [
            '#626262',
            '#626262',
            '#456e40',
            '#456e40',
            '#0078ff',
            '#0078ff',
            '#0078ff',
            '#8f7aac',
            '#8f7aac',
            '#d49a68',
            '#cf3700',
        ];
        const weaponRarityColors = ['#626262', '#456e40', '#0078ff', '#8f7aac', '#d49a68'];
        return { handleChange, valueDisplay, value, characters, fetterColors, weaponRarityColors };
    },
});
</script>

<style lang="less" scoped>
.useInfo {
    * {
        box-sizing: content-box;
    }
    display: flex;
    #output {
        width: 1080px;
        .main {
            background-repeat: no-repeat;
            background-position: top;
            background-size: cover;
            background-position-y: -100px;
            background-color: hsl(90deg 3% 86%);
            position: relative;
            .world {
                position: absolute;
                z-index: 2;
                bottom: 0px;
                left: 20px;
                .uid {
                    font-size: 40px;
                }
                .ts {
                    font-size: 50px;
                    font-weight: bold;
                }
            }
            .stats {
                position: absolute;
                width: 300px;
                right: 0px;
                bottom: 0px;
                text-align: right;
                .item {
                    padding: 0px 50px;
                    background: rgb(2, 0, 36);
                    background: linear-gradient(
                        90deg,
                        rgba(2, 0, 36, 0) 0%,
                        rgba(255, 255, 255, 0.6601015406162465) 13%,
                        rgba(233, 229, 221, 1) 100%
                    );
                    .n-text {
                        margin-bottom: 0px;
                        font-size: 30px;
                        font-weight: bold;
                    }
                }
            }
        }
        .avatars {
            display: flex;
            flex-flow: wrap;
            align-items: center;
            justify-content: space-around;
            background: hsl(90deg 3% 86%);
            .n-grid {
                padding: 0px 20px;
            }
            .item {
                // width: 100%;
                height: 150px;
                border-radius: 20px;
                margin-bottom: 20px;
                padding: 2px 2px 0px 2px;
                border: 4px solid white;
                position: relative;
                .icon {
                    margin-left: 10px;
                    width: 150px;
                    object-fit: cover;
                }
                .fetter {
                    border-radius: 4px;
                    text-align: center;
                    color: white;
                    width: 60px;
                    position: absolute;
                    bottom: 8px;
                    font-weight: bold;
                    font-size: 18px;
                    left: 55px;
                }
                .level {
                    border-radius: 4px;
                    text-align: center;
                    width: 120px;
                    background: white;
                    font-weight: bold;
                    height: 35px;
                    line-height: 35px;
                    font-size: 20px;
                    vertical-align: middle;
                    position: absolute;
                    top: 20px;
                    right: 5px;
                    .n-text {
                        width: 50px;
                        background: green;
                        color: white;
                        float: right;
                        border-radius: 4px;
                    }
                }

                .weapon {
                    border-radius: 4px;
                    text-align: left;
                    width: 120px;
                    background: white;
                    font-weight: bold;
                    font-size: 20px;
                    position: absolute;
                    top: 60px;
                    right: 5px;
                    display: flex;
                    display: flex;
                    justify-content: space-evenly;
                    align-items: center;
                    img {
                        height: 48px;
                        object-fit: cover;
                        border: 1px solid #eee;
                        border-radius: 4px;
                        // position: absolute;
                    }
                    .des {
                        margin-left: 2px;
                        .lv {
                            border-bottom: 2px solid #eee;
                        }
                    }
                }
                &.four {
                    background: rgb(143, 122, 172);
                    background: linear-gradient(
                        90deg,
                        rgba(143, 122, 172, 1) 0%,
                        rgba(178, 154, 212, 1) 80%,
                        rgba(247, 247, 247, 1) 100%
                    );
                }
                &.five {
                    background: rgb(212, 154, 104);
                    background: linear-gradient(
                        90deg,
                        rgba(212, 154, 104, 1) 0%,
                        rgba(238, 184, 138, 1) 80%,
                        rgba(247, 247, 247, 1) 100%
                    );
                }
            }
        }
    }
    #input {
        width: calc(100% - 1080px);
    }
}
</style>
