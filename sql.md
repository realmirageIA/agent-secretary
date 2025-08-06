```

-- Usuários (executivo)
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email TEXT UNIQUE NOT NULL,
  name TEXT NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Tarefas registradas
CREATE TABLE tasks (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id),
  title TEXT NOT NULL,
  description TEXT,
  status TEXT CHECK (status IN ('pending','done','waiting_info')),
  created_at TIMESTAMP DEFAULT NOW(),
  due_date DATE
);

-- Logs de e-mails processados
CREATE TABLE email_logs (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id),
  sender TEXT,
  subject TEXT,
  body TEXT,
  action_taken TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Eventos externos sincronizados (agenda)
CREATE TABLE calendar_events (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id),
  external_id TEXT UNIQUE, -- ID do evento no Google Calendar
  title TEXT,
  start_date TIMESTAMP,
  end_date TIMESTAMP,
  status TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);
```
