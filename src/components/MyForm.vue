<template>
    <div class="form__container">
        <form class="form" id="form" onsubmit="return false">
            <div class="label__row form__label">
                <div>
                    <label for="nameinput" >Наименование товара</label>
                </div>
                <div class="red__point">
                    <svg width="4" height="4" viewBox="0 0 4 4" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <rect width="4" height="4" rx="2" fill="#FF8484"/>
                    </svg>
                </div>
            </div>
            <input
                    class="form__input"
                    placeholder="Ведите наименование товара"
                    id="nameinput"
                    type="text"
                    v-model="formData.name"
                    @input="enableButton(formData)"
                    :class="{form__input__invalid: invalid === true}"
                    required
            />
            <span v-if="formData.name === '' && invalid === true" class="invalid__message">Поле является обязательным</span>
            <label class="form__label" for="descriptioninput" >Описание товара</label>
            <textarea
                    class="form__textarea"
                    placeholder="Ведите описание товара"
                    :class="{textarea__default: textDefault}"
                    @click="textDefaultMethod(formData)"
                    rows="30"
                    cols="30"
                    id="descriptioninput"
                    v-model="formData.description"
                    @input="enableButton(formData)"
            ></textarea>
            <div class="label__row form__label">
                <div>
                    <label for="linkinput" >Ссылка на изображение товара</label>
                </div>
                <div class="red__point">
                    <svg width="4" height="4" viewBox="0 0 4 4" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <rect width="4" height="4" rx="2" fill="#FF8484"/>
                    </svg>
                </div>
            </div>
            <input
                    class="form__input"
                    placeholder="Ведите ссылку"
                    id="linkinput"
                    type="text"
                    v-model="formData.link"
                    @input="enableButton(formData)"
                    :class="{form__input__invalid: invalid === true}"
                    required
            />
            <span v-if="formData.link === '' && invalid === true" class="invalid__message">Поле является обязательным</span>
            <div class="label__row form__label">
                <div>
                    <label for="priceinput" >Цена товара</label>
                </div>
                <div class="red__point">
                    <svg width="4" height="4" viewBox="0 0 4 4" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <rect width="4" height="4" rx="2" fill="#FF8484"/>
                    </svg>
                </div>
            </div>
            <input
                    class="form__input"
                    placeholder="Ведите цену"
                    id="priceinput"
                    v-model="formData.price"
                    @input="enableButton(formData)"
                    v-maska="{ mask: 'Z*M*.M*Z*', tokens: { 'Z': { pattern: /[0-9]/ }, 'M': { pattern: /\s/ }}}"
                    :class="{form__input__invalid: invalid === true}"
                    required
            />
            <span v-if="formData.price === '' && invalid === true" class="invalid__message">Поле является обязательным</span>
            <button
                    class="form__button"
                    id="button"
                    :disabled="!valid"
                    @click="createItem"
            >
                Добавить товар
            </button>
        </form>
    </div>
</template>
<script>
import { maska } from 'maska'
export default {
    name: 'my-form',
    directives: { maska },
    props:{
        formData: {
            type: Object,
            required: {
                type: Boolean,
                default: true
            }
        }
    },
    data () {
      return {
          valid: false,
          invalid: false,
          textDefault: true,
      }
    },
    methods:{
        enableButton (formData) {
            if (Object.values(formData).every(value => !!value)){
                this.valid = true
            } else {
                this.invalid = true
            }
        },
        createItem () {
            if (document.getElementById('form').checkValidity() === true) {
                this.invalid = false
                this.$emit('create')
            }
        },
        textDefaultMethod (formData) {
            if(this.textDefault = true){
                formData.description = '';
            }
            this.textDefault = false
        }
    },
}
</script>
<style lang="sass" scoped>
@mixin button
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1)
    border-radius: 10px
    padding: 10px 0px 10px 0px
    margin-top: 24px
    border: transparent

@mixin input
    background: #FFFEFB
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1)
    border-radius: 4px
    padding: 10px 16px 10px 16px
    border: transparent
    font-style: normal
    font-weight: 400
    font-size: 12px
    line-height: 15px

.form__container
    width: 100%

.form
    display: flex
    flex-direction: column
    background: #FFFEFB
    box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02)
    border-radius: 4px
    padding: 24px
.form__input
    @include input
    height: 16px
.form__label
    font-style: normal
    font-weight: 400
    font-size: 10px
    line-height: 13px
    letter-spacing: -0.02em
    color: #49485E
    margin-bottom: 4px
    margin-top: 16px

.form__textarea
    @include input
    height: 76px
    resize: none
    font-family: inherit
.form__input::placeholder
        color: #B4B4B4
.form__textarea::placeholder
    color: #B4B4B4
.form__button
    color: white
    background: #7BAE73
    @include button
.form__button:disabled
    background: #EEEEEE
    color: #B4B4B4
.form__button:not([disabled]):hover
    transition: 0.2s
    background: limegreen
    color: white
.label__row
    position: relative
    display: flex
    flex-direction: row
    width: max-content
.red__point
    margin-top: -5px
.form__input__invalid
    border: 1px solid red
.form__input:valid
    border: 1px solid #7BAE73
.invalid__message
    font-weight: 400
    font-size: 8px
    line-height: 10px
    letter-spacing: -0.02em
    color: #FF8484
    margin-top: 4px
.textarea__default
    color: #B4B4B4

</style>