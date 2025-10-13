<script lang="ts">
  const links: {
    name: string;
    link: string;
    avatar: string;
    descr: string;
  }[] = [
    {
      name: "Asuka Minato",
      link: "https://asukaminato.notion.site/",
      avatar: "https://avatars.githubusercontent.com/u/30024051?v=4",
      descr: "湊老师,是很重要的启蒙恩师",
    },
    {
      name: "non_hana",
      link: "https://caelum.moe/",
      avatar: "https://avatars.githubusercontent.com/u/105428118?v=4",
      descr: "hana,前端的启蒙,很厉害的全栈DEV",
    },
    {
      name: "Kerman",
      link: "https://github.com/kermanx",
      avatar: "https://avatars.githubusercontent.com/u/63178754?v=4",
      descr: "Kerman,让我见到了学术和基架的天宫一角,很照顾我",
    },
    {
      name: "Shinnku Nikaidou",
      link: "https://nkd.red/",
      avatar: "https://avatars.githubusercontent.com/u/74663709?v=4",
      descr: "真红姐姐,让我见到了大大的世界",
    },
    {
      name: "夏沫花火zzz",
      link: "https://blog.1l1.icu/",
      avatar: "https://avatars.githubusercontent.com/u/79651409?v=4",
      descr: "花火老师,一位前端和运维上的天才",
    },
    {
      name: "yurzhang",
      link: "https://yurzhang.com/",
      avatar: "https://avatars.githubusercontent.com/u/53526247?v=4",
      descr: "yur哥哥,给了我铭记在心的开导",
    },
    {
      name: "Alice Aierlanta",
      link: "https://blog.aierlanta.net",
      avatar: "https://avatars.githubusercontent.com/u/90670661?v=4",
      descr: "很会炼涩图和用llm的alice",
    },
    {
      name: "KUN",
      link: "https://soft.moe/",
      avatar: "https://avatars.githubusercontent.com/u/89312077?v=4",
      descr: "故事的起点",
    },
    {
      name: "Haruki Nikaidou",
      link: "https://blog.plr.moe",
      avatar: "https://avatars.githubusercontent.com/u/135048882?v=4",
      descr: "话糙理不糙的春希",
    },
  ];

  function handleError(e: Event) {
    (e.target as HTMLImageElement).src = "/image/avatar.jpg";
  }
</script>

<div class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-2">
  {#each links as item}
    <a
      id="link-card"
      href={item.link}
      class="flehover)] active:bg-[var(--btn-plain-bg-active)] p-4 hover:pl-6 transition-all"
    >
      <img
        src={item.avatar}
        alt={item.name}
        class="rounded-full flex-0"
        width={64}
        height={64}
        on:error={handleError}
      />

      <div
        class="flex flex-col w-full overflow-hidden text-ellipsis whitespace-nowrap flex-1"
      >
        <div class="text-bold text-xl text-bold text-[var(--primary)]">
          {item.name}
        </div>
        <div class="text-md dark:text-neutral-50 transition" title={item.descr}>
          {item.descr}
        </div>
      </div>
    </a>
  {/each}
</div>

<style>
  #link-card:hover {
    background-color: var(--btn-plain-bg-hover);
    transition-duration: 0.25s;
  }
</style>
