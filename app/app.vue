<template>
    <ClientOnly>
        <div class="min-h-screen bg-gradient-to-br from-primary-50 to-blue-50 pb-6">
            <div class="container mx-auto px-3 sm:px-4 py-4 sm:py-6 max-w-6xl">
                <!-- 头部 -->
                <header class="text-center mb-6 sm:mb-8">
                    <div class="flex items-center justify-center gap-2 sm:gap-3 mb-2">
                        <FileText class="w-6 h-6 sm:w-8 sm:h-8 text-primary-600" />
                        <h1 class="text-2xl sm:text-3xl font-bold text-gray-800">销售出货单生成器</h1>
                    </div>
                    <p class="text-sm sm:text-base text-gray-600">快速创建专业的销售单据</p>
                </header>

                <!-- 表单区域 -->
                <div class="bg-white rounded-xl sm:rounded-2xl shadow-xl p-4 sm:p-6 mb-4 sm:mb-6">
                    <!-- 基本信息 -->
                    <section class="mb-5 sm:mb-6">
                        <h2 class="text-lg sm:text-xl font-semibold text-gray-800 mb-3 sm:mb-4 flex items-center gap-2">
                            <User class="w-4 h-4 sm:w-5 sm:h-5 text-primary-600" />
                            基本信息
                        </h2>
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 sm:gap-4">
                            <div>
                                <label class="block text-xs sm:text-sm font-medium text-gray-700 mb-1">客户名称</label>
                                <input v-model="invoice.customerName" type="text" placeholder="请输入客户名称"
                                    class="w-full px-3 sm:px-4 py-2 sm:py-2.5 text-sm sm:text-base border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-transparent transition" />
                            </div>
                            <div>
                                <label class="block text-xs sm:text-sm font-medium text-gray-700 mb-1">付款方式</label>
                                <select v-model="invoice.paymentMethod"
                                    class="w-full px-3 sm:px-4 py-2 sm:py-2.5 text-sm sm:text-base border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-transparent transition">
                                    <option>现金</option>
                                    <option>转账</option>
                                    <option>支票</option>
                                    <option>月结</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs sm:text-sm font-medium text-gray-700 mb-1">运输方式</label>
                                <input v-model="invoice.shippingMethod" type="text" placeholder="请输入运输方式"
                                    class="w-full px-3 sm:px-4 py-2 sm:py-2.5 text-sm sm:text-base border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-transparent transition" />
                            </div>
                            <div>
                                <label class="block text-xs sm:text-sm font-medium text-gray-700 mb-1">发货单号</label>
                                <input v-model="invoice.invoiceNumber" type="text" placeholder="自动生成"
                                    class="w-full px-3 sm:px-4 py-2 sm:py-2.5 text-sm sm:text-base border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-transparent transition" />
                            </div>
                            <div class="sm:col-span-2">
                                <label class="block text-xs sm:text-sm font-medium text-gray-700 mb-1">发货日期时间</label>
                                <input v-model="invoice.date" type="datetime-local"
                                    class="w-full px-3 sm:px-4 py-2 sm:py-2.5 text-sm sm:text-base border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-transparent transition" />
                            </div>
                        </div>
                    </section>

                    <!-- 产品明细 -->
                    <section class="mb-5 sm:mb-6">
                        <h2 class="text-lg sm:text-xl font-semibold text-gray-800 mb-3 sm:mb-4 flex items-center gap-2">
                            <ShoppingCart class="w-4 h-4 sm:w-5 sm:h-5 text-primary-600" />
                            产品明细
                        </h2>

                        <div class="space-y-3">
                            <div v-for="(item, index) in invoice.items" :key="index"
                                class="bg-gray-50 rounded-lg p-3 sm:p-4 relative">
                                <button v-if="invoice.items.length > 1" @click="removeItem(index)"
                                    class="absolute top-2 right-2 text-red-500 hover:text-red-700 transition active:scale-95 z-10">
                                    <Trash2 class="w-4 h-4 sm:w-5 sm:h-5" />
                                </button>

                                <div class="grid grid-cols-1 gap-2 sm:gap-3 pr-8">
                                    <div class="grid grid-cols-2 gap-2">
                                        <div>
                                            <label
                                                class="block text-xs sm:text-sm font-medium text-gray-700 mb-1">产品编号</label>
                                            <input v-model="item.productCode" type="text" placeholder="如: 000295"
                                                class="w-full px-2 sm:px-3 py-2 text-sm sm:text-base border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-transparent transition" />
                                        </div>
                                        <div>
                                            <label
                                                class="block text-xs sm:text-sm font-medium text-gray-700 mb-1">产品名称</label>
                                            <input v-model="item.name" type="text" placeholder="如: 阿米巴"
                                                class="w-full px-2 sm:px-3 py-2 text-sm sm:text-base border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-transparent transition" />
                                        </div>
                                    </div>

                                    <div class="grid grid-cols-2 gap-2">
                                        <div>
                                            <label
                                                class="block text-xs sm:text-sm font-medium text-gray-700 mb-1">规格</label>
                                            <input v-model="item.specs" type="text" placeholder="如: 62宽方形钛灰1.2米"
                                                class="w-full px-2 sm:px-3 py-2 text-sm sm:text-base border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-transparent transition" />
                                        </div>
                                        <div>
                                            <label
                                                class="block text-xs sm:text-sm font-medium text-gray-700 mb-1">颜色</label>
                                            <input v-model="item.color" type="text" placeholder="如: 白光"
                                                class="w-full px-2 sm:px-3 py-2 text-sm sm:text-base border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-transparent transition" />
                                        </div>
                                    </div>

                                    <div class="grid grid-cols-5 gap-2">
                                        <div>
                                            <label
                                                class="block text-xs sm:text-sm font-medium text-gray-700 mb-1">单位</label>
                                            <select v-model="item.unit"
                                                class="w-full px-1 sm:px-2 py-2 text-xs sm:text-base border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-transparent transition">
                                                <option>PCS</option>
                                                <option>米</option>
                                                <option>套</option>
                                                <option>箱</option>
                                            </select>
                                        </div>
                                        <div>
                                            <label
                                                class="block text-xs sm:text-sm font-medium text-gray-700 mb-1">数量</label>
                                            <input v-model.number="item.quantity" type="number" min="1" placeholder="1"
                                                class="w-full px-1 sm:px-2 py-2 text-xs sm:text-base border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-transparent transition" />
                                        </div>
                                        <div>
                                            <label
                                                class="block text-xs sm:text-sm font-medium text-gray-700 mb-1">单价</label>
                                            <input v-model.number="item.price" type="number" min="0" step="0.1"
                                                placeholder="0.0"
                                                class="w-full px-1 sm:px-2 py-2 text-xs sm:text-base border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-transparent transition" />
                                        </div>
                                        <div>
                                            <label
                                                class="block text-xs sm:text-sm font-medium text-gray-700 mb-1">件数</label>
                                            <input v-model.number="item.packages" type="number" min="0" placeholder="0"
                                                class="w-full px-1 sm:px-2 py-2 text-xs sm:text-base border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-transparent transition" />
                                        </div>
                                        <div>
                                            <label
                                                class="block text-xs sm:text-sm font-medium text-gray-700 mb-1">金额</label>
                                            <div
                                                class="px-1 sm:px-2 py-2 bg-primary-50 rounded-lg text-primary-700 font-semibold text-xs sm:text-base flex items-center justify-center">
                                                {{ (item.quantity * item.price).toFixed(0) }}
                                            </div>
                                        </div>
                                    </div>

                                    <div>
                                        <label
                                            class="block text-xs sm:text-sm font-medium text-gray-700 mb-1">备注</label>
                                        <input v-model="item.notes" type="text" placeholder="如: 60W"
                                            class="w-full px-2 sm:px-3 py-2 text-sm sm:text-base border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-transparent transition" />
                                    </div>
                                </div>
                            </div>
                        </div>

                        <button @click="addItem"
                            class="mt-3 sm:mt-4 w-full py-3 sm:py-3.5 bg-primary-100 text-primary-700 rounded-lg hover:bg-primary-200 active:scale-98 transition-all flex items-center justify-center gap-2 font-semibold text-sm sm:text-base border-2 border-primary-300 hover:border-primary-400 shadow-sm">
                            <Plus class="w-5 h-5 sm:w-5 sm:h-5" />
                            <span>添加产品</span>
                        </button>
                    </section>

                    <!-- 总计信息 -->
                    <section class="bg-gradient-to-r from-primary-50 to-blue-50 rounded-lg p-3 sm:p-4">
                        <div class="space-y-1.5 sm:space-y-2">
                            <div class="flex justify-between text-gray-700 text-sm sm:text-base">
                                <span>合计数量：</span>
                                <span class="font-semibold">{{ totalQuantity }} 件</span>
                            </div>
                            <div class="flex justify-between text-gray-700 text-sm sm:text-base">
                                <span>合计件数：</span>
                                <span class="font-semibold">{{ totalPackages }} 件</span>
                            </div>
                            <div class="h-px bg-gray-300 my-2"></div>
                            <div class="flex justify-between text-lg sm:text-xl font-bold text-primary-700">
                                <span>合计金额：</span>
                                <span>¥ {{ totalAmount.toFixed(0) }}</span>
                            </div>
                            <div class="text-xs sm:text-sm text-gray-600 mt-2">
                                大写：{{ numberToChinese(totalAmount) }}元整
                            </div>
                        </div>
                    </section>

                    <!-- 操作按钮 -->
                    <div class="mt-5 sm:mt-6 flex flex-col sm:flex-row gap-3 sm:gap-4">
                        <button @click="showPreview = !showPreview"
                            style="background-color: #475569; color: white; min-height: 48px;"
                            class="w-full sm:flex-1 py-3 sm:py-3.5 rounded-lg hover:bg-slate-800 active:scale-98 transition-all flex items-center justify-center gap-2 font-semibold text-sm sm:text-base shadow-md hover:shadow-lg">
                            <Eye class="w-5 h-5 sm:w-5 sm:h-5" />
                            <span style="color: white;">{{ showPreview ? '隐藏预览' : '预览销售单' }}</span>
                        </button>
                        <button @click="generatePDF" :disabled="isGenerating"
                            style="background-color: #10b981; color: white; min-height: 48px;"
                            class="w-full sm:flex-1 py-3 sm:py-3.5 rounded-lg hover:bg-emerald-700 active:scale-98 transition-all flex items-center justify-center gap-2 font-semibold text-sm sm:text-base shadow-md hover:shadow-lg disabled:opacity-50 disabled:cursor-not-allowed disabled:shadow-none">
                            <Download class="w-5 h-5 sm:w-5 sm:h-5" />
                            <span style="color: white; font-weight: 600;">{{ isGenerating ? '生成中...' : '生成 PDF'
                                }}</span>
                        </button>
                    </div>
                </div>

                <!-- 预览区域 -->
                <div v-if="showPreview" class="bg-white rounded-xl sm:rounded-2xl shadow-xl p-4 sm:p-6 mb-4 sm:mb-6"
                    ref="invoicePreview">
                    <div class="max-w-5xl mx-auto">
                        <!-- 公司抬头 -->
                        <div class="text-center border-b-2 border-gray-900 pb-2 mb-2">
                            <h1 class="text-lg sm:text-xl md:text-2xl font-bold text-gray-900 mb-1">常州飞象照明设备有限公司</h1>
                            <p class="text-[10px] sm:text-xs text-gray-600">地址：常州市钟楼区邹区镇工业大道62-1佳美大厦3楼304室
                                电话:18015031227/18015031272</p>
                        </div>

                        <!-- 标题和基本信息 -->
                        <div class="mb-2">
                            <div class="flex justify-between items-start text-xs sm:text-sm">
                                <div class="flex-1">
                                    <h2 class="text-base sm:text-lg font-bold text-gray-900 mb-1">销售出货单</h2>
                                    <div class="space-y-0.5">
                                        <div>客户名称：<span class="font-semibold">{{ invoice.customerName }}</span></div>
                                        <div>付款方式：<span class="font-semibold">{{ invoice.paymentMethod }}</span></div>
                                    </div>
                                </div>
                                <div class="text-right">
                                    <div class="mb-1">第1页，共1页</div>
                                    <div>发货单号：<span class="font-semibold">{{ invoice.invoiceNumber }}</span></div>
                                    <div>发货日期：<span class="font-semibold">{{ formatDateTime(invoice.date) }}</span>
                                    </div>
                                    <div>运输方式：<span class="font-semibold">{{ invoice.shippingMethod }}</span></div>
                                </div>
                            </div>
                        </div>

                        <!-- 产品表格 -->
                        <div class="mb-2 overflow-x-auto">
                            <table class="w-full border-collapse border border-gray-900 text-[10px] sm:text-xs">
                                <thead>
                                    <tr class="bg-gray-100">
                                        <th class="border border-gray-900 px-1 py-1 text-center w-8">NO.</th>
                                        <th class="border border-gray-900 px-1 py-1 text-center w-16">产品编号</th>
                                        <th class="border border-gray-900 px-1 py-1 text-center w-16">产品名称</th>
                                        <th class="border border-gray-900 px-1 py-1 text-center">规格</th>
                                        <th class="border border-gray-900 px-1 py-1 text-center w-12">颜色</th>
                                        <th class="border border-gray-900 px-1 py-1 text-center w-10">单位</th>
                                        <th class="border border-gray-900 px-1 py-1 text-center w-12">数量</th>
                                        <th class="border border-gray-900 px-1 py-1 text-center w-12">单价</th>
                                        <th class="border border-gray-900 px-1 py-1 text-center w-16">金额</th>
                                        <th class="border border-gray-900 px-1 py-1 text-center w-12">件数</th>
                                        <th class="border border-gray-900 px-1 py-1 text-center w-16">备注</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="(item, index) in invoice.items" :key="index">
                                        <td class="border border-gray-900 px-1 py-1 text-center">{{ index + 1 }}</td>
                                        <td class="border border-gray-900 px-1 py-1 text-center">{{ item.productCode }}
                                        </td>
                                        <td class="border border-gray-900 px-1 py-1 text-center">{{ item.name }}</td>
                                        <td
                                            class="border border-gray-900 px-1 py-1 text-center text-[9px] sm:text-[10px]">
                                            {{ item.specs }}</td>
                                        <td class="border border-gray-900 px-1 py-1 text-center">{{ item.color }}</td>
                                        <td class="border border-gray-900 px-1 py-1 text-center">{{ item.unit }}</td>
                                        <td class="border border-gray-900 px-1 py-1 text-center font-semibold">{{
                                            item.quantity }}</td>
                                        <td class="border border-gray-900 px-1 py-1 text-center">{{
                                            item.price.toFixed(1) }}</td>
                                        <td class="border border-gray-900 px-1 py-1 text-center font-semibold">{{
                                            (item.quantity * item.price).toFixed(0) }}</td>
                                        <td class="border border-gray-900 px-1 py-1 text-center">{{ item.packages }}
                                        </td>
                                        <td class="border border-gray-900 px-1 py-1 text-center">{{ item.notes }}</td>
                                    </tr>
                                    <!-- 空行填充 -->
                                    <tr v-for="n in Math.max(0, 8 - invoice.items.length)" :key="'empty-' + n">
                                        <td class="border border-gray-900 px-1 py-2">&nbsp;</td>
                                        <td class="border border-gray-900 px-1 py-2"></td>
                                        <td class="border border-gray-900 px-1 py-2"></td>
                                        <td class="border border-gray-900 px-1 py-2"></td>
                                        <td class="border border-gray-900 px-1 py-2"></td>
                                        <td class="border border-gray-900 px-1 py-2"></td>
                                        <td class="border border-gray-900 px-1 py-2"></td>
                                        <td class="border border-gray-900 px-1 py-2"></td>
                                        <td class="border border-gray-900 px-1 py-2"></td>
                                        <td class="border border-gray-900 px-1 py-2"></td>
                                        <td class="border border-gray-900 px-1 py-2"></td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>

                        <!-- 合计行 -->
                        <div class="mb-2 text-[10px] sm:text-xs">
                            <div class="flex items-center gap-2 mb-1 font-semibold">
                                <span>合计：(大写){{ numberToChinese(totalAmount) }}元整</span>
                                <span class="ml-auto">{{ totalQuantity }}</span>
                                <span class="ml-2">¥ {{ totalAmount.toFixed(0) }}</span>
                                <span class="ml-2">共 {{ totalPackages }} 件</span>
                            </div>
                            <div class="text-[9px] sm:text-[10px] text-gray-700 leading-relaxed">
                                备注：凡自提或送货上门的货物，需公司收到货物三日内发现数量、质量有问题，于七日内以书面方式通知我方，逾期不提的，视为货物收妥合格。收货无误。
                            </div>
                            <div class="text-[9px] sm:text-[10px] text-gray-700 mt-0.5">
                                农行卡号：6228 2704 1644 0196 279 微信：18015031227 支付宝：18015031227
                            </div>
                        </div>

                        <!-- 签名栏 -->
                        <div class="grid grid-cols-5 gap-2 text-[10px] sm:text-xs border-t border-gray-400 pt-2">
                            <div>
                                <span class="font-semibold">制单：</span>
                                <span class="inline-block w-12 border-b border-gray-500"></span>
                            </div>
                            <div>
                                <span class="font-semibold">审核：</span>
                                <span class="inline-block w-12 border-b border-gray-500"></span>
                            </div>
                            <div>
                                <span class="font-semibold">仓管：</span>
                                <span class="inline-block w-12 border-b border-gray-500"></span>
                            </div>
                            <div>
                                <span class="font-semibold">送货人：</span>
                                <span class="inline-block w-12 border-b border-gray-500"></span>
                            </div>
                            <div>
                                <span class="font-semibold">收货签字：</span>
                                <span class="inline-block w-12 border-b border-gray-500"></span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </ClientOnly>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import { FileText, User, ShoppingCart, Plus, Trash2, Eye, Download } from 'lucide-vue-next'

interface InvoiceItem {
    productCode: string
    name: string
    specs: string
    color: string
    unit: string
    quantity: number
    price: number
    packages: number
    notes: string
}

interface Invoice {
    customerName: string
    paymentMethod: string
    shippingMethod: string
    invoiceNumber: string
    date: string
    items: InvoiceItem[]
}

const invoice = ref < Invoice > ({
    customerName: '',
    paymentMethod: '现金',
    shippingMethod: '自提',
    invoiceNumber: '',
    date: '',
    items: [
        { productCode: '', name: '', specs: '', color: '', unit: 'PCS', quantity: 1, price: 0, packages: 0, notes: '' }
    ]
})

const showPreview = ref(false)
const isGenerating = ref(false)
const invoicePreview = ref < HTMLElement | null > (null)

// 计算总数量
const totalQuantity = computed(() => {
    return invoice.value.items.reduce((sum, item) => sum + item.quantity, 0)
})

// 计算总金额
const totalAmount = computed(() => {
    return invoice.value.items.reduce((sum, item) => sum + (item.quantity * item.price), 0)
})

// 计算总件数
const totalPackages = computed(() => {
    return invoice.value.items.reduce((sum, item) => sum + item.packages, 0)
})

// 添加项目
const addItem = () => {
    invoice.value.items.push({
        productCode: '',
        name: '',
        specs: '',
        color: '',
        unit: 'PCS',
        quantity: 1,
        price: 0,
        packages: 0,
        notes: ''
    })
}

// 删除项目
const removeItem = (index: number) => {
    if (invoice.value.items.length > 1) {
        invoice.value.items.splice(index, 1)
    }
}

// 生成销售单号
const generateInvoiceNumber = () => {
    const date = new Date()
    const year = date.getFullYear()
    const month = String(date.getMonth() + 1).padStart(2, '0')
    const day = String(date.getDate()).padStart(2, '0')
    const time = String(date.getHours()).padStart(2, '0') + String(date.getMinutes()).padStart(2, '0')
    return `FH${year}${month}${day}${time}`
}

// 格式化日期时间
const formatDateTime = (dateStr: string) => {
    if (!dateStr) return ''
    const date = new Date(dateStr)
    const year = date.getFullYear()
    const month = String(date.getMonth() + 1).padStart(2, '0')
    const day = String(date.getDate()).padStart(2, '0')
    const hours = String(date.getHours()).padStart(2, '0')
    const minutes = String(date.getMinutes()).padStart(2, '0')
    const seconds = String(date.getSeconds()).padStart(2, '0')
    return `${year}/${month}/${day} ${hours}:${minutes}:${seconds}`
}

// 数字转中文大写
const numberToChinese = (num: number) => {
    const digits = ['零', '壹', '贰', '叁', '肆', '伍', '陆', '柒', '捌', '玖']
    const units = ['', '拾', '佰', '仟', '万', '拾万', '佰万', '仟万']

    if (num === 0) return '零'

    let result = ''
    let numStr = Math.floor(num).toString()
    let hasZero = false

    for (let i = 0; i < numStr.length; i++) {
        const digit = parseInt(numStr[i])
        const unitIndex = numStr.length - 1 - i

        if (digit === 0) {
            hasZero = true
        } else {
            if (hasZero) {
                result += '零'
                hasZero = false
            }
            result += digits[digit] + units[unitIndex]
        }
    }

    return result
}

// 生成 PDF
const generatePDF = async () => {
    if (!invoicePreview.value) {
        showPreview.value = true
        await new Promise(resolve => setTimeout(resolve, 100))
    }

    if (!invoicePreview.value) {
        alert('预览区域未加载，请先点击预览按钮')
        return
    }

    isGenerating.value = true

    try {
        // 动态导入 html2pdf.js（仅在客户端）
        const html2pdf = (await import('html2pdf.js')).default

        const opt = {
            margin: 5,
            filename: `销售出货单_${invoice.value.invoiceNumber}.pdf`,
            image: { type: 'jpeg', quality: 0.98 },
            html2canvas: {
                scale: 3,
                useCORS: true,
                letterRendering: true,
                logging: false
            },
            jsPDF: {
                unit: 'mm',
                format: 'a4',
                orientation: 'portrait'
            }
        }

        await html2pdf().set(opt).from(invoicePreview.value).save()
    } catch (error) {
        console.error('PDF 生成失败:', error)
        alert('PDF 生成失败，请重试')
    } finally {
        isGenerating.value = false
    }
}

// 初始化
onMounted(() => {
    invoice.value.invoiceNumber = generateInvoiceNumber()
    const now = new Date()
    const year = now.getFullYear()
    const month = String(now.getMonth() + 1).padStart(2, '0')
    const day = String(now.getDate()).padStart(2, '0')
    const hours = String(now.getHours()).padStart(2, '0')
    const minutes = String(now.getMinutes()).padStart(2, '0')
    invoice.value.date = `${year}-${month}-${day}T${hours}:${minutes}`
})
</script>

<style scoped>
/* 确保打印时的样式 */
@media print {
    body {
        background: white;
    }
}

/* 移动端优化 */
input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
}

input[type="number"] {
    -moz-appearance: textfield;
}

/* 触摸优化 */
button {
    -webkit-tap-highlight-color: transparent;
    touch-action: manipulation;
}

input,
button,
select {
    -webkit-appearance: none;
    appearance: none;
}

/* 按钮增强 */
button:not(:disabled) {
    cursor: pointer;
}

button:focus-visible {
    outline: 2px solid #14b8a6;
    outline-offset: 2px;
}

/* 确保按钮文字可见 */
button span {
    color: inherit !important;
    font-weight: 600 !important;
    display: inline-block;
}

/* 确保按钮背景色可见 */
button.bg-emerald-600 {
    background-color: #059669 !important;
}

button.bg-emerald-600:hover {
    background-color: #047857 !important;
}

button.bg-slate-700 {
    background-color: #334155 !important;
}

button.bg-slate-700:hover {
    background-color: #1e293b !important;
}

/* 平滑滚动 */
html {
    scroll-behavior: smooth;
}

/* 活动状态缩放 */
.active\:scale-98:active {
    transform: scale(0.98);
}

/* PDF 预览区域优化 */
#invoicePreview table {
    page-break-inside: avoid;
}
</style>
