
Next.js 앱 라우터는 React와 웹 플랫폼을 기반으로 한 데이터 펫칭 시스템을 제공함. 데이터의 수명 주기를 관리하는 기본 개념과 패턴을 제공함.

- **서버에서 데이터 가져오기**: 서버 컴포넌트를 사용하여 서버에서 데이터를 가져옴. 이는 백엔드 데이터 리소스에 직접 액세스하고 응용 프로그램의 안전성을 향상시킴.
- **병렬 및 순차 데이터 가져오기**: 병렬로 데이터를 가져와 로딩 시간을 최소화함. 순차 데이터 가져오기는 종속성이 있는 요청 간의 워터폴 효과를 생성.
- **레이아웃과 페이지에 대한 자동 데이터 가져오기 요청 중복 처리**: 레이아웃과 페이지에서 데이터를 가져올 때 요청 중복을 자동으로 처리하여 성능을 향상.
- **`fetch() API`**: `fetch() API`를 기반으로 한 데이터 가져오기 시스템을 제공하며, async/await를 사용하여 서버 컴포넌트에서 데이터를 가져올 수 있음.
- **정적 및 동적 데이터 가져오기**: 정적 데이터는 빌드 시간에 가져와서 캐시되고 재사용되며, 동적 데이터는 각 요청마다 가져올 수 있음.
- **캐싱과 재유효화**: 데이터 캐싱을 통해 요청마다 데이터를 다시 가져오지 않고 성능을 향상시킬 수 있으며, 재유효화를 통해 최신 데이터를 쉽게 업데이트할 수 있음.
- **스트리밍과 서스펜스**: UI의 점진적인 렌더링과 데이터 로딩을 통해 사용자 경험을 개선할 수 있음.

기존의 `getServerSideProps`, `getStaticProps`, `getInitialProps`와 같은 방법은 새로운 App Router에서는 지원되지 않지만, Pages Router에서는 여전히 사용할 수 있음

**## 서버 컴포넌트에서의 async와 await

서버 컴포넌트에서 데이터를 가져오기 위해 async와 await를 사용할 수 있습니다.

```tsx
async function getData() {
  const res = await fetch('https://api.example.com/...')
  // 반환 값은 *직렬화되지 않습니다*
  // Date, Map, Set 등을 반환할 수 있습니다.
 
  // 추천: 오류 핸들링
  if (!res.ok) {
    // 가장 가까운 `error.js` 오류 경계를 활성화합니다.
    throw new Error('데이터를 가져오는 데 실패했습니다.')
  }
 
  return res.json()
}
 
export default async function Page() {
  const data = await getData()
 
  return <main></main>
}
```


### 병렬 데이터 가져오기

클라이언트-서버 워터폴(네트워크 요청이 동기적, 순차적으로 이루어지는 패턴)을 최소화하기 위해, 데이터를 병렬로 가져오는 이 패턴을 권장합니다:

```tsx
import Albums from './albums'
 
async function getArtist(username: string) {
  const res = await fetch(`https://api.example.com/artist/${username}`)
  return res.json()
}
 
async function getArtistAlbums(username: string) {
  const res = await fetch(`https://api.example.com/artist/${username}/albums`)
  return res.json()
}
 
export default async function Page({
  params: { username },
}: {
  params: { username: string }
}) {
  // 두 요청을 병렬로 시작합니다.
  const artistData = getArtist(username)
  const albumsData = getArtistAlbums(username)
 
  // promise가 모두 해결될 때까지 기다립니다.
  const [artist, albums] = await Promise.all([artistData, albumsData])
 
  return (
    <>
      <h1>{artist.name}</h1>
      <Albums list={albums}></Albums>
    </>
  )
}
```


데이터를 순차적으로 가져오려면, 필요한 컴포넌트 내에서 직접 가져오거나, 필요한 컴포넌트 내에서 fetch의 결과를 await할 수 있습니다:

```ts
// ...
 
async function Playlists({ artistID }: { artistID: string }) {
  // 플레이리스트를 기다립니다.
  const playlists = await getArtistPlaylists(artistID)
 
  return (
    <ul>
      {playlists.map((playlist) => (
        <li key={playlist.id}>{playlist.name}</li>
      ))}
    </ul>
  )
}
 
export default async function Page({
  params: { username },
}: {
  params: { username: string }
}) {
  // 아티스트를 기다립니다.
  const artist = await getArtist(username)
 
  return (
    <>
      <h1>{artist.name}</h1>
      <Suspense fallback={<div>Loading...</div>}>
        <Playlists artistID={artist.id} />
      </Suspense>
    </>
  )
}
```

컴포넌트 내에서 데이터를 가져오면, 각 fetch 요청과 루트의 중첩된 세그먼트는 이전 요청 또는 세그먼트가 완료될 때까지 데이터를 가져오고

렌더링을 시작할 수 없습니다.

Next.js에서 데이터 가져오기는 `fetch() API`와 React 서버 컴포넌트를 기반으로 한다. 이를 통해 비동기 함수를 사용하여 React 컴포넌트에서 직접 데이터를 가져올 수 있다.

- **서버 컴포넌트**: 데이터를 가져오기 위해 async와 await를 사용할 수 있음. 이를 통해 데이터를 직접 가져오고 오류를 처리할 수 있음.
    
- **클라이언트 컴포넌트**: 'use'라는 새로운 React 함수를 사용하여 promise를 처리할 수 있음. 현재로서는 클라이언트 컴포넌트에서 fetch를 use로 감싸는 것은 권장되지 않음.
    
- **데이터 가져오기 패턴**: 병렬 데이터 가져오기를 통해 클라이언트-서버 워터폴을 최소화할 수 있음. 순차적 데이터 가져오기는 필요한 컴포넌트 내에서 직접 데이터를 가져오는 방식.
    
- **캐싱**: fetch는 기본적으로 데이터를 자동으로 가져오고 무기한으로 캐시. 캐시된 데이터를 재검증하려면, fetch()에서 next.revalidate 옵션을 사용할 수 있음.
    
- **세그먼트 캐시 구성**: 세그먼트의 기본 캐싱 동작에 의존하거나 세그먼트 캐시 구성을 사용하여 캐싱 또는 재검증 동작을 제어할 수 있음.

# fetch

Next.js는 각 서버 요청마다 영속적인 캐싱을 설정할 수 있도록 기본 `Web fetch() API`를 확장합니다.

브라우저에서는 `cache` 옵션을 사용하여 fetch 요청이 브라우저의 HTTP 캐시와 상호작용하는 방법을 나타냅니다. 이 확장 기능을 사용하면 `cache`는 서버 측 fetch 요청이 프레임워크의 영속적인 HTTP 캐시와 상호작용하는 방법을 나타냅니다.

Server Components 내에서는 `async`와 `await`를 사용하여 `fetch`를 직접 호출할 수 있습니다.

```ts
// app/page.tsx
export default async function Page() {
  // 이 요청은 수동으로 무효화될 때까지 캐시됩니다.
  // `getStaticProps`와 유사합니다.
  // `force-cache`가 기본값이며 생략할 수 있습니다.
  const staticData = await fetch(`https://...`, { cache: 'force-cache' })
 
  // 이 요청은 매 요청마다 다시 가져와야 합니다.
  // `getServerSideProps`와 유사합니다.
  const dynamicData = await fetch(`https://...`, { cache: 'no-store' })
 
  // 이 요청은 10초 동안 캐시되어야 합니다.
  // `getStaticProps`의 `revalidate` 옵션과 유사합니다.
  const revalidatedData = await fetch(`https://...`, {
    next: { revalidate: 10 },
  })
 
  return <div>...</div>
}
```

fetch는 Next.js에서 서버의 각 요청마다 영속적인 캐싱을 설정할 수 있도록 하는 기능

- Next.js는 Web fetch() API를 확장하여 서버 측 fetch 요청이 프레임워크의 영속적인 HTTP 캐시와 상호작용할 수 있도록 함.
- fetch 함수는 Server Components 내에서 async와 await를 사용하여 호출할 수 있음.
- fetch 함수는 다양한 옵션을 받을 수 있고, options.cache 옵션은 요청이 Next.js의 HTTP 캐시와 상호작용하는 방법을 설정.
- options.cache에는 'force-cache'와 'no-store' 두 가지 옵션이 있고, 각각 캐시 사용 여부와 캐시 업데이트 방식을 나타냄.
- options.next.revalidate 옵션은 리소스의 캐시 지속 시간을 설정. 옵션 값으로 false, 0, 또는 양의 정수를 사용할 수 있음.