# open-education-ug
Second option for Open education Uganda Still work in progress
# Open Education Uganda

An open-source educational platform for Ugandan students to access academic materials, with subscription-based access to specific programs and years.

## Features
- Hierarchical navigation (Levels > Colleges > Departments > Programs > Years > Semesters > Courses > Units > Topics).
- Semester-based subscriptions via Mobile Money (Airtel, MTN).
- In-app material viewing with `react-pdf` (web) or `react-native-pdf` (mobile).
- Favorites system for courses.
- Admin dashboard for course management.
- PWA support for offline access and mobile installation.
- Accessible UI with ARIA roles, keyboard/touch navigation, and high-contrast mode.
- Responsive design for all devices (mobile, tablet, desktop).
- TypeScript for type safety.
- Supabase backend for user data, subscriptions, and materials.

## Setup
1. **Clone Repository**:
   ```bash
   git clone https://github.com/your-username/open-education-uganda.git
   cd open-education-uganda
   ```
2. **Install Dependencies**:
   ```bash
   npm install
   ```
3. **Set Up Supabase**:
   - Create a Supabase project at [supabase.com](https://supabase.com).
   - Create tables:
     ```sql
     CREATE TABLE users (
       id UUID PRIMARY KEY,
       name TEXT NOT NULL,
       date_of_birth DATE,
       school TEXT,
       program TEXT,
       phone TEXT,
       email TEXT UNIQUE
     );
     CREATE TABLE subscriptions (
       id UUID PRIMARY KEY,
       user_id UUID REFERENCES users(id),
       level TEXT,
       year TEXT,
       semester TEXT,
       expiry_date DATE
     );
     CREATE TABLE favorites (
       id UUID PRIMARY KEY,
       user_id UUID REFERENCES users(id),
       course_code TEXT
     );
     CREATE TABLE courses (
       id UUID PRIMARY KEY,
       level TEXT,
       college TEXT,
       department TEXT,
       program TEXT,
       year TEXT,
       semester TEXT,
       name TEXT,
       code TEXT,
       description TEXT,
       materials JSONB
     );
     ```
   - Create a `materials` bucket in Supabase Storage with policy:
     ```sql
     CREATE POLICY "Authenticated users can read materials" ON storage.objects
     FOR SELECT TO authenticated USING (bucket_id = 'materials');
     ```
   - Copy `.env.example` to `.env` and add your Supabase credentials:
     ```env
     VITE_SUPABASE_URL=your-supabase-url
     VITE_SUPABASE_ANON_KEY=your-supabase-anon-key
     ```
4. **Run Development Server**:
   ```bash
   npm run dev
   ```
5. **Run Tests**:
   ```bash
   npm run test
   ```
6. **Build for Production**:
   ```bash
   npm run build
   ```

## Mobile App Conversion
- For a native mobile app, initialize a React Native project:
  ```bash
  npx react-native init OpenEducationUganda
  npm install @supabase/supabase-js react-native-pdf @react-navigation/native
  ```
- Reuse Supabase backend and context logic.
- Replace `react-pdf` with `react-native-pdf` in `MaterialItem.tsx`.
- Use `@react-navigation/native` for navigation.

## Contributing
- Fork the repository.
- Create a feature branch: `git checkout -b feature-name`.
- Commit changes: `git commit -m "Add feature-name"`.
- Push and create a pull request.

## License
MIT License
