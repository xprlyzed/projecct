<script>
import AppLayout from '@/Layouts/AppLayout.vue';
export default { layout: AppLayout };
</script>

<script setup>
import { Head, Link, router, usePage } from '@inertiajs/vue3';
import { computed } from 'vue';
import OrderProgress from '@/Components/OrderProgress.vue';
import OrderTimeline from '@/Components/OrderTimeline.vue';

const props = defineProps({ order: Object });
const o = props.order;
const page = usePage();
const flash = computed(() => page.props.flash || {});

function resolve(decision) {
    const msg = decision === 'buyer' ? 'Alıcıya iade edilsin mi?' : 'Ödeme satıcıya aktarılsın mı?';
    if (window.confirm(msg)) {
        router.post(o.resolve_url, { decision }, { preserveScroll: true });
    }
}
</script>

<template>
    <Head :title="`Sipariş ${o.order_number}`" />
    <div class="dash-wrap py-4">
        <div class="admin-toolbar dash-hero">
            <div>
                <div class="toolbar-title">Sipariş {{ o.order_number }}</div>
                <div class="dash-hero-sub">{{ o.auction_title }}</div>
            </div>
            <Link :href="o.index_url" class="btn-admin-ghost"><i class="bi bi-arrow-left"></i> Geri</Link>
        </div>



        <div class="admin-card il-8e86974b">
            <OrderProgress :steps="o.progress_steps" :cancelled="o.is_cancelled"
                           :status-color="o.status_color" :status-icon="o.status_icon" :status-label="o.status_label" />
        </div>

        <div class="ord-grid">
            <div>
                <div v-if="o.status === 'disputed'" class="ord-box il-4afd2f09" data-testid="admin-dispute-box">
                    <div class="ord-box-title il-124741b6"><i class="bi bi-exclamation-octagon"></i> Anlaşmazlık Çözümü</div>
                    <div class="ord-info-row"><span class="k">Alıcının şikayeti</span></div>
                    <p class="pf-text-muted-sm il-d0fa8b10">{{ o.dispute_reason }}</p>
                    <div class="il-871a87af">
                        <button @click="resolve('buyer')" class="btn-admin-danger il-da5cd676" data-testid="resolve-buyer-btn">Alıcı Lehine (İade)</button>
                        <button @click="resolve('seller')" class="btn-admin-pri il-da5cd676" data-testid="resolve-seller-btn">Satıcı Lehine (Öde)</button>
                    </div>
                </div>

                <div class="ord-box">
                    <div class="ord-box-title"><i class="bi bi-info-circle"></i> Sipariş Bilgileri</div>
                    <div class="ord-info-row"><span class="k">Alıcı</span><span class="v">{{ o.buyer_name }}</span></div>
                    <div class="ord-info-row"><span class="k">Satıcı</span><span class="v">{{ o.seller_name }}</span></div>
                    <div class="ord-info-row"><span class="k">Emanet Durumu</span><span class="v">{{ o.escrow_status }}</span></div>
                    <div class="ord-info-row"><span class="k">Kargo</span><span class="v">{{ o.carrier ? o.carrier + ' • ' + o.tracking_number : '—' }}</span></div>
                    <div v-if="o.has_shipping_address" class="ord-info-row"><span class="k">Adres</span><span class="v il-f66b1f3f">{{ o.recipient_name }}, {{ o.shipping_address }}, {{ o.address_city }}</span></div>
                </div>

                <div class="ord-box">
                    <div class="ord-box-title"><i class="bi bi-clock-history"></i> Zaman Çizelgesi</div>
                    <OrderTimeline :events="o.events" />
                </div>
            </div>

            <div>
                <div class="ord-box">
                    <img class="il-6c86cc5e" :src="o.cover_url" alt="">
                    <div class="il-a07b2691">{{ o.auction_title }}</div>
                    <div class="ord-info-row il-5371db16"><span class="k">Tutar</span><span class="v">{{ o.amount }}</span></div>
                    <div class="ord-info-row"><span class="k">Komisyon</span><span class="v">{{ o.commission_amount }}</span></div>
                    <div class="ord-info-row"><span class="k">Durum</span><span class="v" :style="{ color: o.status_color }">{{ o.status_label }}</span></div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped src="../../../../css/pages/admin-orders-show.css"></style>
