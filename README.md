O desafio consiste no desenvolvimento de uma interface web de dashboard.

- Todas as rotas, exceto a de login, deverão estar protegidas por email e senha. Para essa autenticação será usado o Firebase Authentication.
- Deverá ter uma página de listagem de "empresas", contendo as colunas ID, nome, email, telefone e ações
  - As ações possíveis serão "editar" e "deletar"
- Para o layout do site, dê preferência para utilizar a biblioteca TailwindCSS
- Dê preferência para utilizar o Create React App com template Typescript
- Todas as requisições para o Firebase deverá ficar separado em uma pasta service, como por exemplo `src/services/firebase/authentication/login.ts` com sua implementação parecida com o exemplo abaixo. O service só poderá retornar um erro genérico (com uma mensagem), nunca um erro emitido pelo Firebase, pois o componente não deve "saber" que há o Firebase por trás.

```tsx
type LoginDto = {
   email: string
   password: string
}

export default async function login({email, password}: LoginDto): Promise<void> {
   try {
      await firebase.auth().createUserWithEmailAndPassword(email, password)
   } catch(err) {
      throw new Error('There was an error trying to login')
   }
}
```

- Para consumir os services criados dentro do componente, como por exemplo na listagem das empresas, deverá ser utilizado a biblioteca React Query, como no exemplo abaixo:

```tsx
// query
const {data, isLoading, isError} = useQuery(['companies'], getCompanies)

// mutation
const {mutateAsync, data} = useMutation(login)
const handleSubmit = useCallback(async (email: string, password: string) => {
   try {
      await mutateAsync({email, password})
   } catch(err) {
      alert(err.message)
   }
}, [mutateAsync])
```

- Para inserir os dados da empresa, deverá utilizar o Firebase Firestore. Não é necessário adicionar permissões corretas, poderá deixar tudo "aberto" para testes. Ele servirá somente para remover a necessidade do desenvolvimento de um backend.
- O armazenamento do login deverá ser feito preferencialmente através da Context API do React e não pelo Redux.
- Na edição da empresa, deverá utilizar o React Hook Form para trabalhar com formulários. Para validação poderá escolher o Joi ou Yup. Minha recomendação é o Yup pela facilidade de controle das mensagens de erro, porém o Joi é mais completo, porém mais difícil de personalizar o erro.