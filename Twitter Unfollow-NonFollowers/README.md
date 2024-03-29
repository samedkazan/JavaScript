## Geri dönüş yapmayanları takipten çıkarma

## Kılavuz

## Twitter'da takipçi sayınızı düzenlemek bu kod blogu ile artık çok basit. Sadece yapmanız gereken tarayıcı konsoluna girerek bu kodu yapıştırmak. Ardından geri takip etmeyen kullanıcıları ise tek seferde hepsini takip etmeyi bırakmanızı kolaylaştıran bir kod blogu sunuyorum.

## Kullanım

1. Twitter'da tüm takipçileri takibi bırakmak (Sizi takip etmeyenler) <br/>
**Adım 1** : Twitter hesabınıza giriş yapın. <br/>
**Adım 2** : Takip ettikleriniz listesine gidin, _www.twitter.com/following_ <br/>
**Adım 3** : Tarayıcı konsolunu açın. Konsolu açmak için <br/>
 Google Chrome'da `ctrl + shift + J` | <br/>
 Firefox'ta `ctrl + shift + K` | <br/>
 Safari'de `ctrl + alt + I` . <br/>
**Adım 4** : Tüm takip ettiğiniz kullanıcıların listesi yüklenene kadar sayfanın altına otomatik olarak kaydırmak için, aşağıdaki kodu konsola yapıştırın.

[Kod linki](https://github.com/samedkazan/JavaScript/blob/main/Twitter%20Unfollow-NonFollowers/index.js)

```javascript
const delay = () => new Promise(proceed => setTimeout(proceed, 2000));

const scrollDown = async () => {
    const prevScrollTop = document.documentElement.scrollTop;

    window.scrollTo(0, document.body.scrollHeight);
    await delay();

    if (document.documentElement.scrollTop === prevScrollTop) {
        return console.log('%cDone!', 'color:green');
    }

    return twitterUnfollowAllWithoutMutualSubscription();
};

const twitterUnfollowAllWithoutMutualSubscription = async () => {
    const FOLLOWS_TEXT_EN = 'Follows you';
    const FOLLOWS_TEXT_TR = 'Seni takip ediyor';
    const unfollowButtons = document.querySelectorAll('[role="button"][data-testid*="unfollow"]');

    if (!unfollowButtons.length) {
        return console.log('%cList is empty!', 'color:red');
    }

    for (const button of Array.from(unfollowButtons)) {
        const spans = button.parentElement.previousElementSibling.querySelectorAll('span');

        if (!spans.length) continue;

        const nicknameIndex = Array.from(spans).findIndex(span => span.innerText.match('@'));
        const followsMeSpanIndex = Array.from(spans).findIndex(
            span => span.innerText === FOLLOWS_TEXT_EN || span.innerText === FOLLOWS_TEXT_TR
        );

        if (!nicknameIndex && !followsMeSpanIndex) continue;

        const nickname = spans[nicknameIndex];
        const isFollowMe = spans[followsMeSpanIndex];

        if (isFollowMe) continue;

        button.click();

        const confirmationButton = document.querySelector('[data-testid="confirmationSheetConfirm"]');

        if (!confirmationButton) continue;

        confirmationButton.click();

        console.log(`Unfollowed %c${nickname.innerText}!`, 'color:blue');
    }

    await delay();
    return scrollDown();
};

twitterUnfollowAllWithoutMutualSubscription();
```


