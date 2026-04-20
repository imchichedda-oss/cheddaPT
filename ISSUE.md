## .hero
1. .card-btns>.watch-btn 추가해
   https://drive.google.com/file/d/1ZC2h7mAg9HtZhVnQj11e7CLTNBYCpbst/view?usp=drive_link _blank
   이력서보기
2. .hero-video loop
3. .introduce-line-kr, .introduce-line-en 같은 위치에서 2회  무한 루프
   1.  <script src="https://cdn.jsdelivr.net/npm/gsap@3/dist/gsap.min.js"></script>
   2.  gsap.registerPlugin(SplitText);

      const split1 = SplitText.create(".introduce-line-en", {
        type: "chars, words",
      });

      const split2 = SplitText.create(".introduce-line-kr", {
        type: "chars, words",
      });

      gsap.from(split1.chars, {
        y: 50,
        opacity: 0,
        stagger: 0.05,
        duration: 0.6,
        ease: "back.out",
      });
      gsap.from(split2.chars, {
        y: 50,
        opacity: 0,
        stagger: 0.05,
        duration: 0.6,
        ease: "back.out",
      });
  1. 안해봤다는 이유로 안한 적은 없습니다
  2. IM CHAE HYUN, I'M CHALLENGER
4. .card-body
   1. .card-date에 :before{content:'작업기간'}
5. .card-date 다음 형제요소 추가
   1. spen.card-percent
   2. 콘텐츠 100% 삽입