<template>
  <div class="flex flex-col w-full h-full">
    <!-- loading -->
    <div v-if="request.pending.value"
      class="top-0 left-0 w-full h-screen fixed z-50 bg-base-350/40 flex justify-center items-center">
      <ToolsLoading class="w-32 h-32" />
    </div>

    <!-- Tabs -->
    <div class="flex flex-row items-center gap-3 px-2 h-[7%] bg-base-200">
      <TabItem to="/money-pages" :active="true">
        {{ config.by_route(`${current_page}/tabs/list`) }}
      </TabItem>
      <!-- <TabSeparator />
      <TabItem>
        {{ config.by_route(`${current_page}/tabs/analytics`) }}
      </TabItem> -->
    </div>

    <!--  body -->
    <div class="w-full h-[93%]">
      <!-- text -->
      <div class="w-full h-24 flex items-end justify-center">
        <div
          class="w-[96%] h-[70px] rounded-lg border border-base-400 bg-base-200 flex items-center justify-center text-sm p-4">
          صفحات پول‌ساز، بخش‌هایی از وبسایت هستند که به طور مستقیم یا غیرمستقیم به درآمد کسب و کار کمک می‌کنند، این
          صفحات به صورت مستقیم به فروش محصولات یا خدمات شما می‌پردازند و مشتریان را به انجام خرید تشویق می‌کنند.
        </div>
      </div>
      <!-- add page -->
      <div class="w-full h-16 flex items-center justify-between px-5 mt-2">
        <p class="text-lg">لیست صفحات تجاری من</p>
        <button class="w-40 h-10 btn-primary" disabled>
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <mask id="mask0_6223_9102" style="mask-type: alpha" maskUnits="userSpaceOnUse" x="0" y="0" width="24"
              height="24">
              <rect width="24" height="24" />
            </mask>
            <g mask="url(#mask0_6223_9102)">
              <path
                d="M12 18.75C11.7833 18.75 11.6043 18.6793 11.463 18.538C11.321 18.396 11.25 18.2167 11.25 18V12.75H6C5.78333 12.75 5.60433 12.679 5.463 12.537C5.321 12.3957 5.25 12.2167 5.25 12C5.25 11.7833 5.321 11.604 5.463 11.462C5.60433 11.3207 5.78333 11.25 6 11.25H11.25V6C11.25 5.78333 11.321 5.60433 11.463 5.463C11.6043 5.321 11.7833 5.25 12 5.25C12.2167 5.25 12.396 5.321 12.538 5.463C12.6793 5.60433 12.75 5.78333 12.75 6V11.25H18C18.2167 11.25 18.396 11.3207 18.538 11.462C18.6793 11.604 18.75 11.7833 18.75 12C18.75 12.2167 18.6793 12.3957 18.538 12.537C18.396 12.679 18.2167 12.75 18 12.75H12.75V18C12.75 18.2167 12.6793 18.396 12.538 18.538C12.396 18.6793 12.2167 18.75 12 18.75Z" />
            </g>
          </svg>
          افزودن صفحه
        </button>
      </div>
      <!-- sort and filter -->
      <div class="w-full h-20 flex items-end justify-between px-5" v-if="false">
        <select name="" id="" class="w-[439px] h-11 text-base-500 text-sm rounded border border-base-400 px-2">
          <option value="">لطفا یک گزینه را انتخاب کنید</option>
        </select>
        <div class="flex items-center justify-center text-xs">
          <p>مرتب سازی بر اساس</p>
          <select name="" id="" class="w-[284px] text-base-500 h-11 text-sm mr-3 rounded border border-base-400 px-2">
            <option value="">لطفا یک گزینه را انتخاب کنید</option>
          </select>
        </div>
        <button class="w-[72px] h-10 btn-secondary">اعمال</button>
      </div>
      <!-- item -->
      <div class="w-full max-h-[660px] h-fit px-5 flex items-center justify-center">
        <div class="w-full h-full rounded-lg border border-base-400 overflow-y-auto text-sm"
          v-if="Boolean(Object.keys(data).length)">
          <!-- Guide bar -->
          <table class="w-full h-auto bg-base-200 overflow-hidden" dir="rtl">
            <tr class="w-full h-[60px]">
              <td class="h-[60px] w-[246px] max-w-[246px] text-center border-l border-base-400">
                آدرس صفحات URLs
              </td>
              <td class="h-[60px] w-[197px] text-center border-l border-base-400">
                میانگین رتبه کلمات کلیدی
              </td>
              <td class="h-[60px] w-[147px] text-center border-l border-base-400">
                آخرین بروزرسانی
              </td>
              <td class="h-[60px] w-[197px] text-center border-l border-base-400">
                وضعیت صفحه
              </td>
              <td class="h-[60px] w-[76px] text-center border-base-400">جزئیات</td>
            </tr>
          </table>
          <!-- Map on data -->
          <div dir="ltr" class="w-full h-auto max-h-[600px] overflow-hiden">
            <table class="w-full h-auto overflow-auto" dir="rtl">
              <tr v-for="(page, index) in data" :key="index" class="w-full h-[60px]">
                <td :title="page.link"
                  class="h-[60px] w-[246px] max-w-[246px] text-center border-l border-base-400 border-b overflow-hidden"
                  dir="ltr">
                  <a :href="page.link" target="new">{{ show_url(page.link) }}</a>
                </td>
                <td class="h-[60px] w-[197px] text-center border-l border-base-400 border-b">
                  <div class="w-full h-full flex items-center justify-center" v-if="page.positions_avrage === null">
                    <button class="w-16 h-10 btn-secondary" disabled>
                      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <mask id="mask0_6223_2719" style="mask-type: alpha" maskUnits="userSpaceOnUse" x="0" y="0"
                          width="24" height="24">
                          <rect width="24" height="24" />
                        </mask>
                        <g mask="url(#mask0_6223_2719)">
                          <path
                            d="M12 18.75C11.7833 18.75 11.6043 18.6793 11.463 18.538C11.321 18.396 11.25 18.2167 11.25 18V12.75H6C5.78333 12.75 5.60433 12.679 5.463 12.537C5.321 12.3957 5.25 12.2167 5.25 12C5.25 11.7833 5.321 11.604 5.463 11.462C5.60433 11.3207 5.78333 11.25 6 11.25H11.25V6C11.25 5.78333 11.321 5.60433 11.463 5.463C11.6043 5.321 11.7833 5.25 12 5.25C12.2167 5.25 12.396 5.321 12.538 5.463C12.6793 5.60433 12.75 5.78333 12.75 6V11.25H18C18.2167 11.25 18.396 11.3207 18.538 11.462C18.6793 11.604 18.75 11.7833 18.75 12C18.75 12.2167 18.6793 12.3957 18.538 12.537C18.396 12.679 18.2167 12.75 18 12.75H12.75V18C12.75 18.2167 12.6793 18.396 12.538 18.538C12.396 18.6793 12.2167 18.75 12 18.75Z" />
                        </g>
                      </svg>
                    </button>
                  </div>
                  <div v-else>
                    {{ page.positions_average === 0 ? "بدون رتبه" : page.positions_average === null ? "غیرفعال" :
                      page.positions_average }}
                  </div>
                </td>
                <td class="h-[60px] w-[147px] text-center border-l border-base-400 border-b" dir="ltr">
                  <span v-if="page.updated_at === null"> بدون دیتا </span>
                  <span v-else>
                    {{
                      jalaliMoment(page.updated_at, "YYYY-MM-DD HH:mm:ss").format(
                    "jYYYY/jMM/jDD"
                    )
                    }}
                  </span>
                </td>
                <td class="h-[60px] w-[197px] text-[10px] text-end border-l border-base-400 border-b px-6">
                  <ProgressLinear :value="page.page_status">
                    {{ Math.round(page.page_status * 100) / 100 }} درصد
                  </ProgressLinear>
                </td>
                <td class="h-[60px] w-[76px] text-center border-base-400 border-b">
                  <div class="w-full h-full flex items-center justify-center">
                    <NuxtLink :to="{ path: '/money-pages/single', query: { page: page.uuid } }"
                      class="w-16 h-10 btn-secondary">
                      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <mask id="mask0_6223_5077" style="mask-type: alpha" maskUnits="userSpaceOnUse" x="0" y="0"
                          width="24" height="24">
                          <rect width="24" height="24" />
                        </mask>
                        <g mask="url(#mask0_6223_5077)">
                          <path
                            d="M11.0943 19.0896L4.63856 12.6339C4.54498 12.5403 4.47895 12.4416 4.44049 12.3377C4.40204 12.2339 4.38281 12.1217 4.38281 12.0012C4.38281 11.8807 4.40204 11.7685 4.44049 11.6647C4.47895 11.5608 4.54498 11.4621 4.63856 11.3685L11.1039 4.90315C11.2424 4.76468 11.4123 4.69288 11.6136 4.68775C11.8148 4.68263 11.993 4.75763 12.1482 4.91275C12.3033 5.05761 12.3834 5.23069 12.3885 5.43197C12.3937 5.63324 12.3187 5.81144 12.1635 5.96657L6.87889 11.2512H18.4866C18.6994 11.2512 18.8776 11.323 19.0212 11.4666C19.1648 11.6102 19.2366 11.7884 19.2366 12.0012C19.2366 12.214 19.1648 12.3922 19.0212 12.5358C18.8776 12.6794 18.6994 12.7512 18.4866 12.7512H6.87889L12.1732 18.0454C12.3116 18.1839 12.3834 18.3563 12.3885 18.5627C12.3937 18.7691 12.3187 18.9448 12.1635 19.0896C12.0187 19.2448 11.843 19.3223 11.6366 19.3223C11.4302 19.3223 11.2495 19.2448 11.0943 19.0896Z" />
                        </g>
                      </svg>
                    </NuxtLink>
                  </div>
                </td>
              </tr>
            </table>
          </div>
        </div>
      </div>
      <!-- stop button -->
      <div class="w-full h-24 flex items-center justify-end px-5" v-if="false">
        <button class="w-[156px] h-10 btn-danger">
          <svg width="16" height="18" viewBox="0 0 16 18" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path
              d="M3.3 17.5C2.8 17.5 2.375 17.325 2.025 16.975C1.675 16.625 1.5 16.2 1.5 15.7V3H1.25C1.03333 3 0.854333 2.929 0.713 2.787C0.571 2.64567 0.5 2.46667 0.5 2.25C0.5 2.03333 0.571 1.85433 0.713 1.713C0.854333 1.571 1.03333 1.5 1.25 1.5H5C5 1.25 5.08733 1.04167 5.262 0.875C5.43733 0.708333 5.65 0.625 5.9 0.625H10.1C10.35 0.625 10.5627 0.708333 10.738 0.875C10.9127 1.04167 11 1.25 11 1.5H14.75C14.9667 1.5 15.146 1.571 15.288 1.713C15.4293 1.85433 15.5 2.03333 15.5 2.25C15.5 2.46667 15.4293 2.64567 15.288 2.787C15.146 2.929 14.9667 3 14.75 3H14.5V15.7C14.5 16.2 14.325 16.625 13.975 16.975C13.625 17.325 13.2 17.5 12.7 17.5H3.3ZM3 3V15.7C3 15.7833 3.02933 15.8543 3.088 15.913C3.146 15.971 3.21667 16 3.3 16H12.7C12.7833 16 12.8543 15.971 12.913 15.913C12.971 15.8543 13 15.7833 13 15.7V3H3ZM5.4 13.25C5.4 13.4667 5.471 13.646 5.613 13.788C5.75433 13.9293 5.93333 14 6.15 14C6.36667 14 6.546 13.9293 6.688 13.788C6.82933 13.646 6.9 13.4667 6.9 13.25V5.75C6.9 5.53333 6.82933 5.354 6.688 5.212C6.546 5.07067 6.36667 5 6.15 5C5.93333 5 5.75433 5.07067 5.613 5.212C5.471 5.354 5.4 5.53333 5.4 5.75V13.25ZM9.1 13.25C9.1 13.4667 9.17067 13.646 9.312 13.788C9.454 13.9293 9.63333 14 9.85 14C10.0667 14 10.246 13.9293 10.388 13.788C10.5293 13.646 10.6 13.4667 10.6 13.25V5.75C10.6 5.53333 10.5293 5.354 10.388 5.212C10.246 5.07067 10.0667 5 9.85 5C9.63333 5 9.454 5.07067 9.312 5.212C9.17067 5.354 9.1 5.53333 9.1 5.75V13.25ZM3 3V15.7C3 15.7833 3.02933 15.8543 3.088 15.913C3.146 15.971 3.21667 16 3.3 16H3V3Z" />
          </svg>

          توقف رهگیری
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import Config from "~~/composables/Config";
import jalaliMoment from "jalali-moment";
import Request from "~~/Api/Request";

const request = new Request("v1");
const current_page = "pages/money-pages";
const config = new Config();
const data = ref({});

const Sites = useSitesStore();

async function load_data(uuid) {
  if (uuid == null) {
    return
  }
  let res = await request
    .get("money-pages", { workspace: uuid })
    .then((res) => {
      data.value = res.data;
    })
    .catch((err) => {
      console.error(err);
    });
}

function show_url(url) {
  let new_url = url.replace("https://", "").replace(/\/$/, '');
  let slices = new_url.split("/")
  let path = slices.slice(1).join("/")
  return [slices[0], path.length > 15 ? `${path.slice(0, 12)}...` : path].join("/").replace(/\/$/, '');
}

watch(() => Sites.current, (newValue) => {
  load_data(newValue.uuid);
});

onMounted(() => {
  load_data(Sites?.current?.uuid ?? null);
})


</script>
