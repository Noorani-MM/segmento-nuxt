<template>
  <div class="flex flex-col w-full h-full">
    <!-- loading -->
    <div
      v-if="request.pending.value"
      class="top-0 left-0 w-full h-screen fixed z-50 bg-base-350/40 flex justify-center items-center"
    >
      <ToolsLoading class="w-32 h-32" />
    </div>

    <!-- Tabs -->
    <div class="flex flex-row items-center gap-3 px-2 h-[7%] bg-base-200">
      <TabItem to="/idea-generator" :active="true">
        {{ config.by_route(`${current_page}/search/title`) }}
      </TabItem>
    </div>

    <!-- page content -->
    <div
      class="flex flex-col gap-4 p-2 w-full pt-8"
      :class="data.length === 0 ? 'h-full' : 'h-fit'"
    >
      <!-- search box -->
      <form
        @submit.prevent="search_keywords_request()"
        class="flex flex-row items-center w-full gap-2"
      >
        <div class="flex items-center w-full justify-between gap-2 h-10">
          <div class="custom_input_box w-[95%] text-base-500">
            <InputText
              autocomplete="keyword"
              v-model="form.keyword"
              type="text"
              @focus="search_class.focus()"
              @blur="search_class.leave()"
            />
            <label
              class="!text-base-400"
              :class="search_class.transitionStyle(form.keyword, 'text-base-400')"
              >{{ config.by_route(`${current_page}/place-holder`) }}</label
            >
          </div>
          <button type="submit" class="btn-primary w-[5%] h-full">
            <svg
              width="18"
              height="18"
              viewBox="0 0 18 18"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M16.0234 17.05L10.2484 11.3C9.74844 11.7167 9.17344 12.0417 8.52344 12.275C7.87344 12.5083 7.20677 12.625 6.52344 12.625C4.80677 12.625 3.35677 12.0333 2.17344 10.85C0.990104 9.66667 0.398438 8.21667 0.398438 6.5C0.398438 4.8 0.990104 3.354 2.17344 2.162C3.35677 0.970667 4.80677 0.375 6.52344 0.375C8.22344 0.375 9.6651 0.966667 10.8484 2.15C12.0318 3.33333 12.6234 4.78333 12.6234 6.5C12.6234 7.21667 12.5068 7.9 12.2734 8.55C12.0401 9.2 11.7234 9.76667 11.3234 10.25L17.0984 16.025C17.2318 16.1583 17.2984 16.325 17.2984 16.525C17.2984 16.725 17.2234 16.9 17.0734 17.05C16.9234 17.2 16.7444 17.275 16.5364 17.275C16.3278 17.275 16.1568 17.2 16.0234 17.05ZM6.52344 11.125C7.80677 11.125 8.89444 10.675 9.78644 9.775C10.6778 8.875 11.1234 7.78333 11.1234 6.5C11.1234 5.21667 10.6778 4.125 9.78644 3.225C8.89444 2.325 7.80677 1.875 6.52344 1.875C5.22344 1.875 4.12777 2.325 3.23644 3.225C2.34444 4.125 1.89844 5.21667 1.89844 6.5C1.89844 7.78333 2.34444 8.875 3.23644 9.775C4.12777 10.675 5.22344 11.125 6.52344 11.125Z"
                fill="white"
              />
            </svg>
          </button>
        </div>
      </form>

      <!-- page content label -->
      <span v-if="data.length !== 0">
        {{
          config
            .by_route(`${current_page}/search/sentence`)
            [Number(data.length !== 0)].replace("[count]", data.length)
        }}
      </span>

      <!-- page content -->
      <div class="justify-between" :class="data.length === 0 ? 'h-full w-full' : 'h-fit'">
        <!-- form -->
        <div
          class="flex flex-col pt-2 border border-base-400 rounded-[3px]"
          :class="`${data.length === 0 ? 'text-primary-disabled h-full' : 'h-fit'}`"
        >
          <!-- header -->
          <div class="flex flex-row justify-between px-2">
            <div class="flex items-center">
              <span class="w-12 flex justify-center">
                {{ config.by_route(`${current_page}/table/row`) }}
              </span>
              <span class="w-fit">
                {{ config.by_route(`${current_page}/table/result`) }}
              </span>
            </div>
            <div class="flex flex-row items-center gap-4">
              <CopyArray :content="copy_all_content(data)" class="btn-secondary" v-if="data !== null">
                <svg
                  width="17"
                  height="20"
                  viewBox="0 0 17 20"
                  fill="none"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    d="M6.27812 15.8008C5.76146 15.8008 5.32812 15.6258 4.97812 15.2758C4.62812 14.9258 4.45312 14.5008 4.45312 14.0008V2.62578C4.45312 2.10911 4.62812 1.67578 4.97812 1.32578C5.32812 0.975781 5.76146 0.800781 6.27812 0.800781H14.6281C15.1281 0.800781 15.5575 0.979781 15.9161 1.33778C16.2741 1.69645 16.4531 2.12578 16.4531 2.62578V14.0008C16.4531 14.5008 16.2741 14.9258 15.9161 15.2758C15.5575 15.6258 15.1281 15.8008 14.6281 15.8008H6.27812ZM6.27812 14.3008H14.6281C14.7281 14.3008 14.8075 14.2714 14.8661 14.2128C14.9241 14.1548 14.9531 14.0841 14.9531 14.0008V2.62578C14.9531 2.52578 14.9241 2.44645 14.8661 2.38778C14.8075 2.32978 14.7281 2.30078 14.6281 2.30078H6.27812C6.17812 2.30078 6.09913 2.32978 6.04113 2.38778C5.98246 2.44645 5.95312 2.52578 5.95312 2.62578V14.0008C5.95312 14.0841 5.98246 14.1548 6.04113 14.2128C6.09913 14.2714 6.17812 14.3008 6.27812 14.3008ZM2.75312 19.3008C2.25312 19.3008 1.82812 19.1258 1.47812 18.7758C1.12812 18.4258 0.953125 18.0008 0.953125 17.5008V5.55078C0.953125 5.35078 1.02379 5.17578 1.16513 5.02578C1.30713 4.87578 1.48646 4.80078 1.70312 4.80078C1.90312 4.80078 2.07812 4.87578 2.22812 5.02578C2.37812 5.17578 2.45312 5.35078 2.45312 5.55078V17.5008C2.45312 17.5841 2.48213 17.6548 2.54013 17.7128C2.59879 17.7714 2.66979 17.8008 2.75312 17.8008H11.7031C11.9031 17.8008 12.0781 17.8758 12.2281 18.0258C12.3781 18.1758 12.4531 18.3508 12.4531 18.5508C12.4531 18.7674 12.3781 18.9464 12.2281 19.0878C12.0781 19.2298 11.9031 19.3008 11.7031 19.3008H2.75312Z"
                  />
                </svg>
                {{ config.by_route(`${current_page}/table/buttons/copy`) }}
              </CopyArray>
            </div>
          </div>

          <!-- when we haven't content this div will show -->
          <div
            class="flex flex-col items-center justify-center w-full h-full gap-3"
            v-if="data.length === 0"
          >
            <svg
              width="109"
              height="108"
              viewBox="0 0 109 108"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M29.2317 84.584C30.4566 84.584 31.5284 84.1512 32.4471 83.2856C33.3657 82.4159 33.8251 81.3176 33.8251 79.9906V49.3685C33.8251 48.0415 33.3657 46.9432 32.4471 46.0735C31.5284 45.208 30.4566 44.7752 29.2317 44.7752C27.9048 44.7752 26.8085 45.208 25.9429 46.0735C25.0732 46.9432 24.6384 48.0415 24.6384 49.3685V79.9906C24.6384 81.3176 25.0732 82.4159 25.9429 83.2856C26.8085 84.1512 27.9048 84.584 29.2317 84.584ZM52.9639 84.584C54.2909 84.584 55.3892 84.1512 56.2589 83.2856C57.1244 82.4159 57.5572 81.3176 57.5572 79.9906V30.9952C57.5572 29.6682 57.1244 28.572 56.2589 27.7064C55.3892 26.8367 54.2909 26.4019 52.9639 26.4019C51.6369 26.4019 50.5407 26.8367 49.6751 27.7064C48.8054 28.572 48.3706 29.6682 48.3706 30.9952V79.9906C48.3706 81.3176 48.8054 82.4159 49.6751 83.2856C50.5407 84.1512 51.6369 84.584 52.9639 84.584ZM76.6961 84.584C78.023 84.584 79.1213 84.1512 79.991 83.2856C80.8566 82.4159 81.2894 81.3176 81.2894 79.9906V67.7418C81.2894 66.4148 80.8566 65.3165 79.991 64.4468C79.1213 63.5812 78.023 63.1485 76.6961 63.1485C75.4712 63.1485 74.3994 63.5812 73.4808 64.4468C72.5621 65.3165 72.1028 66.4148 72.1028 67.7418V79.9906C72.1028 81.3176 72.5621 82.4159 73.4808 83.2856C74.3994 84.1512 75.4712 84.584 76.6961 84.584ZM11.9302 107.551C8.86801 107.551 6.26513 106.479 4.12158 104.335C1.97803 102.192 0.90625 99.5888 0.90625 96.5266V14.4592C0.90625 11.397 1.97803 8.79414 4.12158 6.65059C6.26513 4.50704 8.86801 3.43526 11.9302 3.43526H63.3754C64.7024 3.43526 65.8007 3.86805 66.6704 4.73364C67.536 5.60331 67.9688 6.70162 67.9688 8.02858C67.9688 9.35554 67.536 10.4518 66.6704 11.3174C65.8007 12.1871 64.7024 12.6219 63.3754 12.6219H11.9302C11.4199 12.6219 10.9871 12.8016 10.6318 13.1609C10.2725 13.5161 10.0929 13.9489 10.0929 14.4592V96.5266C10.0929 97.037 10.2725 97.4718 10.6318 97.8311C10.9871 98.1863 11.4199 98.3639 11.9302 98.3639H93.9976C94.508 98.3639 94.9428 98.1863 95.3021 97.8311C95.6573 97.4718 95.8349 97.037 95.8349 96.5266V45.0814C95.8349 43.7544 96.2677 42.6561 97.1333 41.7864C98.003 40.9209 99.1013 40.4881 100.428 40.4881C101.755 40.4881 102.854 40.9209 103.723 41.7864C104.589 42.6561 105.022 43.7544 105.022 45.0814V96.5266C105.022 99.5888 103.95 102.192 101.806 104.335C99.6627 106.479 97.0598 107.551 93.9976 107.551H11.9302ZM91.2416 34.0574C89.9146 34.0574 88.8163 33.6226 87.9466 32.7529C87.0811 31.8873 86.6483 30.791 86.6483 29.4641V21.8086H78.9927C77.6658 21.8086 76.5695 21.3737 75.7039 20.504C74.8343 19.6385 74.3994 18.5422 74.3994 17.2152C74.3994 15.8883 74.8343 14.792 75.7039 13.9264C76.5695 13.0567 77.6658 12.6219 78.9927 12.6219H86.6483V4.96637C86.6483 3.63941 87.0811 2.54109 87.9466 1.67143C88.8163 0.805839 89.9146 0.373047 91.2416 0.373047C92.5686 0.373047 93.6669 0.805839 94.5365 1.67143C95.4021 2.54109 95.8349 3.63941 95.8349 4.96637V12.6219H103.49C104.817 12.6219 105.916 13.0567 106.785 13.9264C107.651 14.792 108.084 15.8883 108.084 17.2152C108.084 18.5422 107.651 19.6385 106.785 20.504C105.916 21.3737 104.817 21.8086 103.49 21.8086H95.8349V29.4641C95.8349 30.791 95.4021 31.8873 94.5365 32.7529C93.6669 33.6226 92.5686 34.0574 91.2416 34.0574Z"
                fill="#D9D9D9"
              />
            </svg>
            <span class="text-base-350">{{
              config.by_route(`${current_page}/table/empty`)
            }}</span>
          </div>

          <!-- when have content this div will show -->
          <div class="flex flex-col px-2" v-else>
            <!-- this must be component -->
            <div>
              <div
                v-for="(item, index) in data"
                class="w-full flex flex-row items-center py-3"
                :class="Number(index) + 1 === data.length ? '' : 'border-b'"
              >
                <!-- table content -->
                <div class="flex flex-col w-full">
                  <!-- row -->
                  <div class="flex flex-row items-center child">
                    <span class="w-12 flex justify-center items-center">
                      {{ Number(index) + 1 }}
                    </span>
                    <span class="w-fit flex justify-center items-center pr-3.5 group">
                      <Copy :content="item" class="[&>svg]:fill-secondary-text flex flex-row gap-2 items-center">
                        <span v-html="item"></span>
                        <svg
                        class="hidden group-hover:block"
                        width="14"
                        height="14"
                        viewBox="0 0 17 20"
                        fill="none"
                        xmlns="http://www.w3.org/2000/svg"
                        >
                        <path
                            d="M6.27812 15.8008C5.76146 15.8008 5.32812 15.6258 4.97812 15.2758C4.62812 14.9258 4.45312 14.5008 4.45312 14.0008V2.62578C4.45312 2.10911 4.62812 1.67578 4.97812 1.32578C5.32812 0.975781 5.76146 0.800781 6.27812 0.800781H14.6281C15.1281 0.800781 15.5575 0.979781 15.9161 1.33778C16.2741 1.69645 16.4531 2.12578 16.4531 2.62578V14.0008C16.4531 14.5008 16.2741 14.9258 15.9161 15.2758C15.5575 15.6258 15.1281 15.8008 14.6281 15.8008H6.27812ZM6.27812 14.3008H14.6281C14.7281 14.3008 14.8075 14.2714 14.8661 14.2128C14.9241 14.1548 14.9531 14.0841 14.9531 14.0008V2.62578C14.9531 2.52578 14.9241 2.44645 14.8661 2.38778C14.8075 2.32978 14.7281 2.30078 14.6281 2.30078H6.27812C6.17812 2.30078 6.09913 2.32978 6.04113 2.38778C5.98246 2.44645 5.95312 2.52578 5.95312 2.62578V14.0008C5.95312 14.0841 5.98246 14.1548 6.04113 14.2128C6.09913 14.2714 6.17812 14.3008 6.27812 14.3008ZM2.75312 19.3008C2.25312 19.3008 1.82812 19.1258 1.47812 18.7758C1.12812 18.4258 0.953125 18.0008 0.953125 17.5008V5.55078C0.953125 5.35078 1.02379 5.17578 1.16513 5.02578C1.30713 4.87578 1.48646 4.80078 1.70312 4.80078C1.90312 4.80078 2.07812 4.87578 2.22812 5.02578C2.37812 5.17578 2.45312 5.35078 2.45312 5.55078V17.5008C2.45312 17.5841 2.48213 17.6548 2.54013 17.7128C2.59879 17.7714 2.66979 17.8008 2.75312 17.8008H11.7031C11.9031 17.8008 12.0781 17.8758 12.2281 18.0258C12.3781 18.1758 12.4531 18.3508 12.4531 18.5508C12.4531 18.7674 12.3781 18.9464 12.2281 19.0878C12.0781 19.2298 11.9031 19.3008 11.7031 19.3008H2.75312Z"
                        />
                        </svg>
                    </Copy>
                    </span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <button
          class="btn-primary mt-2.5 text-white gap-2 px-3"
          v-if="data.length !== 0"
          @click="search_keywords_request(true)"
        >
          <svg
            width="22"
            height="16"
            viewBox="0 0 22 16"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M11.0484 15.75C8.88177 15.75 7.0401 14.996 5.52344 13.488C4.00677 11.9793 3.24844 10.15 3.24844 8V7.225L1.92344 8.55C1.77344 8.68333 1.59844 8.75 1.39844 8.75C1.19844 8.75 1.02344 8.68333 0.873438 8.55C0.723438 8.4 0.648438 8.22067 0.648438 8.012C0.648438 7.804 0.723438 7.63333 0.873438 7.5L3.37344 5C3.45677 4.9 3.55244 4.83333 3.66044 4.8C3.7691 4.76667 3.88177 4.75 3.99844 4.75C4.1151 4.75 4.22777 4.76667 4.33644 4.8C4.44444 4.83333 4.5401 4.9 4.62344 5L7.12344 7.5C7.27344 7.63333 7.34844 7.804 7.34844 8.012C7.34844 8.22067 7.27344 8.4 7.12344 8.55C6.97344 8.68333 6.79844 8.75 6.59844 8.75C6.39844 8.75 6.22344 8.68333 6.07344 8.55L4.74844 7.225V8C4.74844 9.73333 5.3611 11.2083 6.58644 12.425C7.8111 13.6417 9.29844 14.25 11.0484 14.25C11.3818 14.25 11.7191 14.221 12.0604 14.163C12.4024 14.1043 12.7401 14.0167 13.0734 13.9C13.1901 13.8333 13.3234 13.8167 13.4734 13.85C13.6234 13.8833 13.7401 13.95 13.8234 14.05C14.0568 14.2667 14.1484 14.504 14.0984 14.762C14.0484 15.0207 13.8818 15.2 13.5984 15.3C13.1818 15.4333 12.7611 15.5417 12.3364 15.625C11.9111 15.7083 11.4818 15.75 11.0484 15.75ZM17.9984 11.25C17.8818 11.25 17.7694 11.2333 17.6614 11.2C17.5528 11.1667 17.4568 11.1 17.3734 11L14.8734 8.5C14.7234 8.36667 14.6484 8.19567 14.6484 7.987C14.6484 7.779 14.7234 7.6 14.8734 7.45C15.0234 7.31667 15.1984 7.25 15.3984 7.25C15.5984 7.25 15.7734 7.31667 15.9234 7.45L17.2484 8.775V8C17.2484 6.26667 16.6361 4.79167 15.4114 3.575C14.1861 2.35833 12.6984 1.75 10.9484 1.75C10.6151 1.75 10.2778 1.77933 9.93644 1.838C9.59444 1.896 9.25677 1.98333 8.92344 2.1C8.80677 2.16667 8.67344 2.18333 8.52344 2.15C8.37344 2.11667 8.25677 2.05 8.17344 1.95C7.9401 1.73333 7.84844 1.49567 7.89844 1.237C7.94844 0.979 8.1151 0.8 8.39844 0.7C8.8151 0.566666 9.2361 0.458333 9.66144 0.375C10.0861 0.291667 10.5151 0.25 10.9484 0.25C13.1151 0.25 14.9568 1.004 16.4734 2.512C17.9901 4.02067 18.7484 5.85 18.7484 8V8.775L20.0734 7.45C20.2234 7.31667 20.3984 7.25 20.5984 7.25C20.7984 7.25 20.9734 7.31667 21.1234 7.45C21.2734 7.6 21.3484 7.779 21.3484 7.987C21.3484 8.19567 21.2734 8.36667 21.1234 8.5L18.6234 11C18.5401 11.1 18.4441 11.1667 18.3354 11.2C18.2274 11.2333 18.1151 11.25 17.9984 11.25Z"
              fill="white"
            />
          </svg>
          {{ config.by_route(`${current_page}/table/buttons/more`) }}
        </button>
      </div>
    </div>
  </div>
</template>
<script setup>
import Request from "~~/Api/Request";
import Config from "~~/composables/Config";
import Auth from "~~/middlewares/Auth";
import { CustomTextBox } from "~~/composables/CustomTextBox";

const current_page = "pages/idea-generator";
const config = new Config();
const search_class = new CustomTextBox();
const data = ref([]);

const request = new Request("v1");
const form = ref({
  keyword: "",
  limit: 10,
});

definePageMeta({
  middleware: [Auth],
});

function copy_all_content(results) {
  let content = [];
  results.forEach(() => {
    content.push(...results);
  });
  return content;
}

async function search_keywords_request(update) {
  let res = await request
    .get("idea-generator/generate", form.value)
    .then((res) => {
      update ? (data.value = [...data.value, ...res.data]) : (data.value = res.data);
    })
    .catch((err) => {
      console.log(err);
    });
}
</script>
