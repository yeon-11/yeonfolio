<template>
    <v-container class="work-projects-section">
        <!-- 타이틀 -->
        <h2 class="text-h5 font-weight-bold mb-6">
            📂 Work Projects
        </h2>

        <!-- ================= 카테고리 토글 ================= -->
        <div class="category-toggle mb-8">
            <button v-for="category in categories" :key="category.value" type="button" class="category-pill"
                :class="{ 'category-pill--active': selectedCategory === category.value }"
                @click="selectedCategory = category.value">
                {{ category.label }}
            </button>
        </div>

        <!-- ================= 배너 카테고리 전용 멀티 슬라이더 ================= -->
        <div v-if="selectedCategory === 'banner' && bannerGroups.length" class="mb-8">
            <!-- 프로모션 / 상단 이미지 / 자유 배너 그룹 -->
            <div v-for="group in bannerGroups" :key="group.key" class="mb-8">
                <!-- 그룹 라벨 -->
                <div class="banner-group-header">
                    <span class="banner-label">
                        {{ group.label }}
                    </span>
                </div>

                <!-- 그룹별로 다른 사이즈를 줄 수 있도록 key 기반 클래스 추가 -->
                <v-sheet class="banner-slider" :class="`banner-slider--${group.key}`" rounded="lg" elevation="2">
                    <v-window v-model="bannerIndexes[group.key]" show-arrows>
                        <v-window-item v-for="(slide, i) in group.slides" :key="i" :value="i">
                            <!-- 파일명만 저장해두고, 실제 경로는 resolveImage에서 계산 -->
                            <v-img :src="resolveImage(slide.image)" :alt="slide.title" class="banner-image"
                                :class="`banner-image--${group.key}`" cover>
                                <!-- 텍스트 오버레이 -->
                                <div class="banner-caption">
                                    <div class="banner-title">
                                        {{ slide.title }}
                                    </div>
                                    <div class="banner-desc">
                                        {{ slide.description }}
                                    </div>
                                </div>
                            </v-img>
                        </v-window-item>
                    </v-window>

                    <!-- 인디케이터 -->
                    <div class="banner-dots">
                        <button v-for="(_, i) in group.slides" :key="i" type="button" class="slider-dot" :class="{
                            'slider-dot--active': bannerIndexes[group.key] === i,
                        }" @click="bannerIndexes[group.key] = i" />
                    </div>
                </v-sheet>
            </div>
        </div>

        <!-- ================= Masonry 레이아웃 ================= -->
        <div v-if="masonryProjects.length" class="projects-masonry">
            <div v-for="(project, index) in masonryProjects" :key="index" class="masonry-item" :class="[
                'type-' + project.type,
                {
                    // 제품 사진 탭일 때는 '이미지만' 보여주기 위한 전용 클래스
                    'photo-only': selectedCategory === 'photo',
                },
            ]">
                <!-- resolveImage 사용 -->
                <v-img :src="resolveImage(project.image)" :alt="project.title" class="masonry-image" :class="{
                    // 📸 제품 사진 탭일 때는 이미지도 혼자 카드 느낌 나도록 radius 적용
                    'masonry-image--photo-only': selectedCategory === 'photo',
                }" cover />

                <!-- 제품 사진 카테고리에서는 타이틀/설명/메타/태그를 전부 숨김 -->
                <div v-if="selectedCategory !== 'photo'" class="masonry-info">
                    <div class="masonry-title">
                        {{ project.title }}
                    </div>
                    <div class="masonry-desc">
                        {{ project.description }}
                    </div>
                    <div class="masonry-meta">
                        <span v-if="project.role">{{ project.role }}</span>
                        <span v-if="project.role && project.year"> · </span>
                        <span v-if="project.year">{{ project.year }}</span>
                    </div>

                    <div v-if="project.tags && project.tags.length" class="masonry-tags">
                        <span v-for="(tag, tIndex) in project.tags" :key="tIndex" class="masonry-tag">
                            {{ tag }}
                        </span>
                    </div>
                </div>
            </div>
        </div>

        <!-- ================= 결과 없음 안내 ================= -->
        <div v-if="selectedCategory !== 'banner' && masonryProjects.length === 0"
            class="text-body-2 text-center py-10 text-grey-darken-1">
            해당 카테고리에 등록된 작업이 아직 없어요
        </div>
    </v-container>
</template>

<script>
export default {
    name: 'WorkProjects',

    data() {
        return {
            // ================= 카테고리 버튼 =================
            categories: [
                { value: 'all', label: '전체' },
                { value: 'banner', label: '배너' },
                { value: 'photo', label: '제품 사진' },
            ],

            selectedCategory: 'all',

            // 배너 그룹별 현재 슬라이드 인덱스
            bannerIndexes: {},

            // 배너 그룹
            bannerGroups: [],

            // ================= 작업 데이터 =================
            projects: [
                // 1106
                {
                    title: '특수키 제품 촬영컷 01',
                    description: '특수키 제품 촬영 및 사진 보정, 배경 작업',
                    type: 'photo',
                    category: ['photo'],
                    image: 'photo1106(1).jpg',
                    tags: ['제품 사진', '보정'],
                    role: '촬영 · 보정',
                    year: '2025',
                },
                // 1111
                {
                    title: '특수키 제품 촬영컷 02',
                    description: '특수키 제품 촬영 및 사진 보정, 배경 작업',
                    type: 'photo',
                    category: ['photo'],
                    image: 'photo1111(1).jpg',
                    tags: ['제품 사진', '보정'],
                    role: '촬영 · 보정',
                    year: '2025',
                },
                {
                    title: '특수키 제품 촬영컷 03',
                    description: '특수키 제품 촬영 및 사진 보정, 배경 작업',
                    type: 'photo',
                    category: ['photo'],
                    image: 'photo1111(2).jpg',
                    tags: ['제품 사진', '보정'],
                    role: '촬영 · 보정',
                    year: '2025',
                },
                {
                    title: '골드바 포인트 키캡 배너',
                    description: '신상품 프로모션용으로 제작한 배너',
                    type: 'banner',
                    category: ['banner'],
                    image: 'promo_banner1111(1).jpg',
                    tags: ['배너', '프로모션'],
                    role: '디자인 · 편집',
                    year: '2025',
                },
                // 1113
                {
                    title: '시즌 전환 배너 리뉴얼',
                    description: '여름 시즌 배너를 사계절 콘셉트에 맞춰 리디자인한 배너',
                    type: 'banner',
                    category: ['banner'],
                    image: 'promo_banner1113(1).jpg',
                    tags: ['배너', '프로모션'],
                    role: '디자인 · 편집',
                    year: '2025',
                },
                // 1114
                {
                    title: '우미어 전용 배너',
                    description: '우미어 키보드 제품을 중심으로 제작한 배너',
                    type: 'banner',
                    category: ['banner'],
                    image: 'main_banner1114(1).jpg',
                    tags: ['배너'],
                    role: '디자인',
                    year: '2025',
                },
                {
                    title: '커스텀 용품 배너',
                    description: '취향대로 꾸미는 재미를 놀고있는 감성으로 풀어낸 커스텀 배너',
                    type: 'banner',
                    category: ['banner'],
                    image: 'main_banner1114(2).jpg',
                    tags: ['배너'],
                    role: '디자인',
                    year: '2025',
                },
                // 1117
                {
                    title: '일반키 제품 촬영컷 01',
                    description: '일반키 제품 촬영 및 사진 보정, 배경 작업',
                    type: 'photo',
                    date: '1117',
                    category: ['photo'],
                    image: 'photo1117(1).jpg',
                    tags: ['제품 사진', '보정'],
                    role: '촬영 · 보정',
                    year: '2025',
                },
                {
                    title: '일반키 제품 촬영컷 02',
                    description: '일반키 제품 촬영 및 사진 보정, 배경 작업',
                    type: 'photo',
                    date: '1117',
                    category: ['photo'],
                    image: 'photo1117(2).jpg',
                    tags: ['제품 사진', '보정'],
                    role: '촬영 · 보정',
                    year: '2025',
                },
                {
                    title: '일반키 제품 촬영컷 03',
                    description: '일반키 제품 촬영 및 사진 보정, 배경 작업',
                    type: 'photo',
                    date: '1117',
                    category: ['photo'],
                    image: 'photo1117(3).jpg',
                    tags: ['제품 사진', '보정'],
                    role: '촬영 · 보정',
                    year: '2025',
                },
                {
                    title: '커스텀 키캡 배너',
                    description: '커스텀 키캡의 다양한 연출을 담은 배너',
                    type: 'banner',
                    category: ['banner'],
                    image: 'main_banner1118(1).jpg',
                    tags: ['배너'],
                    role: '디자인',
                    year: '2025',
                },
                {
                    title: '일반키 제품 촬영컷 04',
                    description: '일반키 제품 촬영 및 사진 보정, 배경 작업',
                    type: 'photo',
                    date: '1117',
                    category: ['photo'],
                    image: 'photo1117(4).jpg',
                    tags: ['제품 사진', '보정'],
                    role: '촬영 · 보정',
                    year: '2025',
                },
                {
                    title: '일반키 제품 촬영컷 05',
                    description: '일반키 제품 촬영 및 사진 보정, 배경 작업',
                    type: 'photo',
                    date: '1117',
                    category: ['photo'],
                    image: 'photo1117(5).jpg',
                    tags: ['제품 사진', '보정'],
                    role: '촬영 · 보정',
                    year: '2025',
                },
                // 1118
                {
                    title: '테마별 키캡 배너',
                    description: '키캡을 테마별로 분류해 선택 과정을 직관적으로 보여주는 배너',
                    type: 'banner',
                    category: ['banner'],
                    image: 'main_banner1118(2).jpg',
                    tags: ['배너'],
                    role: '디자인',
                    year: '2025',
                },
                {
                    title: '포인트 키캡 배너',
                    description: '포인트 키캡의 개성과 사용성을 귀여운 콘셉트로 표현한 배너',
                    type: 'banner',
                    category: ['banner'],
                    image: 'main_banner1118(3).jpg',
                    tags: ['배너'],
                    role: '디자인',
                    year: '2025',
                },
                {
                    title: '프로파일별 키캡 배너',
                    description: '주요 키캡 프로파일을 보기 쉽게 정리한 선택 가이드 배너',
                    type: 'banner',
                    category: ['banner'],
                    image: 'main_banner1118(4).jpg',
                    tags: ['배너'],
                    role: '디자인',
                    year: '2025',
                },
                {
                    title: '키캡 키링 배너',
                    description: '통일된 구성으로 제작한 주제별 배너',
                    type: 'banner',
                    category: ['banner'],
                    image: 'main_banner1118(5).jpg',
                    tags: ['배너'],
                    role: '디자인',
                    year: '2025',
                },
                // 1119
                {
                    title: '일반키 제품 촬영컷 06',
                    description: '일반키 제품 촬영 및 사진 보정, 배경 작업',
                    type: 'photo',
                    category: ['photo'],
                    image: 'photo1119(1).jpg',
                    tags: ['제품 사진', '보정'],
                    role: '촬영 · 보정',
                    year: '2025',
                },
                {
                    title: '솜데이즈 배너',
                    description: '솜데이즈의 사랑스러운 분위기를 담아 제작한 배너',
                    type: 'banner',
                    category: ['banner'],
                    image: 'main_banner1119(1).jpg',
                    tags: ['배너'],
                    role: '디자인',
                    year: '2025',
                },
                {
                    title: '일반키 제품 촬영컷 07',
                    description: '일반키 제품 촬영 및 사진 보정, 배경 작업',
                    type: 'photo',
                    category: ['photo'],
                    image: 'photo1119(2).jpg',
                    tags: ['제품 사진', '보정'],
                    role: '촬영 · 보정',
                    year: '2025',
                },
                // {
                //     title: '11.19 상단 배너 2',
                //     description: '제품 상세 상단 배너 2',
                //     type: 'banner',
                //     category: ['banner'],
                //     image: 'main_banner1119(2).jpg',
                //     tags: ['배너'],
                //     role: '디자인',
                //     year: '2025',
                // },
                // {
                //     title: '11.19 상단 배너 3',
                //     description: '제품 상세 상단 배너 3',
                //     type: 'banner',
                //     category: ['banner'],
                //     image: 'main_banner1119(3).jpg',
                //     tags: ['배너'],
                //     role: '디자인',
                //     year: '2025',
                // },
                // 1121
                {
                    title: '키링 제품 촬영컷 01',
                    description: '키링 제품 촬영 및 사진 보정 작업',
                    type: 'photo',
                    category: ['photo'],
                    image: 'photo1121(1).jpg',
                    tags: ['제품 사진', '보정'],
                    role: '촬영 · 보정',
                    year: '2025',
                },
                // 1124
                {
                    title: '전체 상품 배너',
                    description: '키캡 디자인의 포인트 색감을 살려 제작한 배너',
                    type: 'banner',
                    category: ['banner'],
                    image: 'main_banner1124(1).jpg',
                    tags: ['배너'],
                    role: '디자인',
                    year: '2025',
                },
                // 1125
                {
                    title: '키링 제품 촬영컷 02',
                    description: '키링 제품 촬영 및 사진 보정 작업',
                    type: 'photo',
                    category: ['photo'],
                    image: 'photo1125(1).jpg',
                    tags: ['제품 사진', '보정'],
                    role: '촬영 · 보정',
                    year: '2025',
                },
                {
                    title: '신제품 배너',
                    description: '새로운 디자인을 소개하는 포인트형 신상품 배너',
                    type: 'banner',
                    category: ['banner'],
                    image: 'free_banner1125(1).png',
                    tags: ['배너'],
                    role: '디자인',
                    year: '2025',
                },
                {
                    title: '일반키 제품 촬영컷 08',
                    description: '일반키 제품 촬영 및 사진 보정, 배경 작업',
                    type: 'photo',
                    category: ['photo'],
                    image: 'photo1125(2).png',
                    tags: ['제품 사진', '보정'],
                    role: '촬영 · 보정',
                    year: '2025',
                },
                // 1126
                {
                    title: '베스트 리뷰 배너',
                    description: '많은 리뷰를 강조하기 위해 리뷰 화면을 기반으로 제작한 배너',
                    type: 'banner',
                    category: ['banner'],
                    image: 'free_banner1126(1).png',
                    tags: ['배너'],
                    role: '디자인',
                    year: '2025',
                },
            ],
        };
    },

    computed: {
        masonryProjects() {
            if (this.selectedCategory === 'banner') return [];
            if (this.selectedCategory === 'all') return this.projects;

            // 카테고리(제품 사진)에서는 해당 type만 필터링
            return this.projects.filter((p) => p.type === this.selectedCategory);
        },
    },

    created() {
        const bannerProjects = this.projects.filter((p) => p.type === 'banner');

        // ---- 이름으로 자동 분류되는 필터 ----
        const promotionSlides = bannerProjects.filter((p) =>
            p.image.startsWith('promo_banner'),
        );
        const mainSlides = bannerProjects.filter((p) =>
            p.image.startsWith('main_banner'),
        );
        const freeSlides = bannerProjects.filter((p) =>
            p.image.startsWith('free_banner'),
        );

        this.bannerGroups = [
            {
                key: 'promotion',
                label: '프로모션',
                slides: promotionSlides,
            },
            {
                key: 'main',
                label: '상단 이미지',
                slides: mainSlides,
            },
            {
                key: 'free',
                label: '자유 배너',
                slides: freeSlides,
            },
        ].filter((g) => g.slides.length > 0);

        // 그룹별 v-window 인덱스 초기화
        this.bannerIndexes = this.bannerGroups.reduce((acc, group) => {
            acc[group.key] = 0;
            return acc;
        }, {});
    },

    mounted() {
        window.scrollTo({ top: 0, behavior: 'auto' });
    },

    methods: {
        resolveImage(fileName) {
            if (!fileName) return '';
            const rawBase = import.meta.env.BASE_URL || '/';
            const base = rawBase.endsWith('/') ? rawBase.slice(0, -1) : rawBase;
            return `${base}/image/work/${fileName}`;
        },
    },
};
</script>

<style scoped>
/* ================= 섹션 타이틀 폰트 ================= */
.work-projects-section h2 {
    font-family: 'SUITE-Bold', sans-serif;
}

/* 섹션 내 텍스트 폰트 통일 */
.work-projects-section p,
.work-projects-section div {
    font-family: 'SUITE-Regular', sans-serif;
}

/* ================= 카테고리 토글 ================= */
.category-toggle {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
}

.category-pill {
    border-radius: 9999px;
    border: 1px solid #d0d4e4;
    padding: 6px 14px;
    background-color: #ffffff;
    font-size: 0.85rem;
    cursor: pointer;
    transition:
        background-color 0.15s ease,
        color 0.15s ease,
        border-color 0.15s ease,
        transform 0.1s ease;
}

.category-pill:hover {
    transform: translateY(-1px);
    border-color: #4f5d75;
}

.category-pill--active {
    background-color: #4f5d75;
    color: #ffffff;
    border-color: #4f5d75;
}

/* ================= 배너 그룹 헤더 ================= */
.banner-group-header {
    margin-bottom: 6px;
    padding-left: 4px;
}

.banner-label {
    font-size: 0.8rem;
    color: #6b7280;
}

/* ================= 배너 슬라이더 공통 ================= */
.banner-slider {
    padding: 0 !important;
    border-radius: 20px;
    overflow: hidden;
    position: relative;
}

/* ================= 배너 이미지 공통 ================= */
.banner-image {
    width: 100%;
    object-fit: contain;
    background-color: #ffffff;
}

/* 실제 이미지 해상도 기준 비율 적용 */
.banner-image--promotion {
    aspect-ratio: 1920 / 400;
}

.banner-image--main {
    aspect-ratio: 1280 / 400;
}

.banner-image--free {
    aspect-ratio: 1280 / 200;
}

/* 텍스트 오버레이 */
.banner-caption {
    position: absolute;
    left: 32px;
    bottom: 28px;
    max-width: 60%;
    color: #ffffff;
    text-shadow: 0 2px 8px rgba(0, 0, 0, 0.5);
}

.banner-title {
    font-size: 1.3rem;
    font-weight: 700;
    margin-bottom: 4px;
}

.banner-desc {
    font-size: 0.9rem;
    opacity: 0.9;
}

/* 슬라이더 모드에서만 텍스트가 사라지고 전체보기에서는 유지 */
.banner-slider .banner-caption {
    display: none !important;
}

/* 슬라이더 인디케이터 */
.banner-dots {
    position: absolute;
    left: 50%;
    bottom: 12px;
    transform: translateX(-50%);
    display: flex;
    align-items: center;
}

.slider-dot {
    width: 6px;
    height: 6px;
    border-radius: 999px;
    border: none;
    margin: 0 3px;
    padding: 0;
    background-color: rgba(255, 255, 255, 0.4);
    cursor: pointer;
}

.slider-dot--active {
    width: 14px;
    background-color: rgba(255, 255, 255, 0.9);
}

/* ================= Masonry 레이아웃 ================= */
.projects-masonry {
    column-count: 2;
    column-gap: 16px;
}

@media (min-width: 960px) {
    .projects-masonry {
        column-count: 3;
    }
}

@media (min-width: 1280px) {
    .projects-masonry {
        column-count: 4;
    }
}

.masonry-item {
    break-inside: avoid;
    margin-bottom: 16px;
    border-radius: 16px;
    overflow: hidden;
    background-color: #f7f7fa;
    box-shadow: 0 2px 6px rgba(15, 23, 42, 0.08);
    display: flex;
    flex-direction: column;
}

.masonry-image {
    width: 100%;
    display: block;
}

/* 텍스트 영역 */
.masonry-info {
    padding: 10px 12px 12px;
}

.masonry-title {
    font-size: 0.9rem;
    font-weight: 700;
    margin-bottom: 2px;
}

.masonry-desc {
    font-size: 0.8rem;
    color: #4b5563;
    margin-bottom: 6px;
}

.masonry-meta {
    font-size: 0.75rem;
    color: #6b7280;
    margin-bottom: 4px;
}

.masonry-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 4px;
}

.masonry-tag {
    font-size: 0.7rem;
    padding: 2px 6px;
    border-radius: 999px;
    background-color: #ffffff;
    border: 1px solid #e5e7eb;
}

/* 제품 사진 전용 */
.masonry-item.photo-only {
    background-color: transparent;
    box-shadow: none;
    border-radius: 16px;
}

.masonry-image--photo-only {
    border-radius: 16px;
}
</style>
